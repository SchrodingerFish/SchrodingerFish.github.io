---
title: Web 应用中线程池的合理管理与销毁
date: 2026-04-25 08:20:11
categories:
  - Web
tags:
  - java
---

# Web 应用中线程池的合理管理与销毁
在 **Servlet（或 Spring MVC）** 项目里，通常会把 **线程池** 当成 *全局资源* 来使用：

* **初始化**：在容器启动时（`contextInitialized`）创建一次；
* **使用**：在 controller（Servlet、Spring Controller）里拿到引用并 `submit` 任务；
* **销毁**：在容器关闭时（`contextDestroyed`）统一、优雅地关闭。

只有把 **创建** 与 **销毁** 放在同一个生命周期管理器里（`ServletContextListener`、Spring Bean 生命周期、Java EE `ManagedExecutorService`），才能避免资源泄漏、线程阻塞、ClassLoader 泄漏等常见问题。

下面给出 **两套** 常用实现方式（纯 Servlet 与 Spring MVC），并详述 **“如何合理销毁线程池”** 的最佳实践。

---

## 1️⃣ 纯 Servlet 方式（基于 `ServletContextListener`）

### 1.1 代码结构

```java
// 1️⃣ Listener：负责创建 & 销毁 ExecutorService
@WebListener                     // 等价于在 web.xml 中 <listener> 声明
public class ThreadPoolListener implements ServletContextListener {

    // 线程池实例
    private ExecutorService executor;

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        // 这里可以根据业务需求选择合适的实现
        int core = Runtime.getRuntime().availableProcessors() * 2;
        this.executor = Executors.newFixedThreadPool(core,
                new ThreadFactoryBuilder()
                        .setNameFormat("app-pool-%d")
                        .setDaemon(false)   // 必须非 daemon，容器会负责 shutdown
                        .build());

        // 把线程池放进 ServletContext，供全局共享
        sce.getServletContext().setAttribute("APP_EXECUTOR", executor);
        log.info("Thread pool created: coreSize={}, executor={}", core, executor);
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        ExecutorService exec = (ExecutorService) sce.getServletContext()
                                                   .getAttribute("APP_EXECUTOR");
        if (exec == null) return;   // 防御式编程

        // ---- ① 先受理已有请求的任务 ----
        exec.shutdown();   // 不再接受新任务

        // ---- ② 等待正在执行的任务结束 ----
        try {
            if (!exec.awaitTermination(30, TimeUnit.SECONDS)) {
                // 超时未结束，强制终止
                List<Runnable> awaiting = exec.shutdownNow();
                log.warn("Force shutdown, {} tasks were awaiting execution", awaiting.size());
                // 再次等待，防止线程仍在阻塞
                if (!exec.awaitTermination(10, TimeUnit.SECONDS)) {
                    log.error("Thread pool did not terminate after forced shutdown");
                }
            }
        } catch (InterruptedException e) {
            // 当前线程被中断 → 立刻尝试强制关闭
            exec.shutdownNow();
            Thread.currentThread().interrupt(); // 保留中断状态
        }

        // ---- ③ 清理 ServletContext 中的引用 ----
        sce.getServletContext().removeAttribute("APP_EXECUTOR");
        log.info("Thread pool shut down cleanly");
    }
}
```

#### 关键点说明

| 步骤                                                         | 为什么                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `@WebListener` / `web.xml` 声明                              | 让容器在 **Web 应用启动/停止** 时自动回调                    |
| `ServletContext#setAttribute`                                | 让 **所有 Servlet/Filter/Controller** 能通过 `getServletContext().getAttribute` 取到同一个实例，避免重复创建 |
| `shutdown()` + `awaitTermination()`                          | **先不接受新任务**，再给已提交的任务一定的宽限期完成         |
| 超时后 `shutdownNow()`                                       | **强制终止** 仍在执行/等待的任务，防止容器阻塞住而导致 *部署/热升级* 失败 |
| 捕获 `InterruptedException` 并 `Thread.currentThread().interrupt()` | 尊重中断信号，避免吞掉中断导致后续业务错误                   |
| `removeAttribute`                                            | 防止因 **ClassLoader 泄漏**（旧的 WebApp 仍残留引用）导致内存无法回收 |

> **Tip**：如果线程池内部的任务本身对 **中断** 有响应（`Thread.interrupted()`、`Future.cancel(true)`），则 `shutdownNow()` 能更干净地结束它们。否则只能“强行杀掉”正在阻塞的 I/O，这可能留下资源泄漏，需要业务代码自行做好 clean‑up。

### 1.2 在 Controller（Servlet）里使用

```java
@WebServlet("/asyncTask")
public class AsyncTaskServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {

        ExecutorService exec = (ExecutorService) req.getServletContext()
                                                   .getAttribute("APP_EXECUTOR");

        if (exec == null) {
            resp.sendError(HttpServletResponse.SC_INTERNAL_SERVER_ERROR,
                           "Executor not initialized");
            return;
        }

        // 业务示例：把耗时任务交给线程池
        exec.submit(() -> {
            try {
                // 这里放业务逻辑 (例如计算、调用外部系统等)
                Thread.sleep(2000); // 模拟耗时
                log.info("Task completed in thread {}", Thread.currentThread().getName());
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt(); // 响应中断
                log.warn("Task was interrupted", e);
            } catch (Exception e) {
                log.error("Unexpected error in async task", e);
            }
        });

        // 立即返回给前端，或使用 AsyncContext 进一步做异步响应
        resp.getWriter().write("Task has been submitted");
    }
}
```

> **注意**：**不要在 Controller 里自行 `shutdown()`**，因为那会把全局线程池一次性关闭，导致后续请求全部失效。**销毁只能交给 Listener**（容器关闭时）或 **Spring Bean 的 `@PreDestroy`**。

---

## 2️⃣ Spring MVC（或 Spring Boot）方式

如果项目已经基于 Spring，推荐把线程池交给 **Spring 管理**，利用 Bean 的生命周期来完成创建与销毁。

### 2.1 通过 `@Configuration` 定义 Bean

```java
@Configuration
public class ExecutorConfig {

    // 采用 Spring 自带的 ThreadPoolTaskExecutor
    @Bean(name = "appExecutor", destroyMethod = "shutdown")
    public ThreadPoolTaskExecutor appExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(Runtime.getRuntime().availableProcessors() * 2);
        executor.setMaxPoolSize(50);
        executor.setQueueCapacity(200);
        executor.setThreadNamePrefix("app-pool-");
        executor.setAwaitTerminationSeconds(30);      // Spring 会在 shutdown 时调用 awaitTermination
        executor.setWaitForTasksToCompleteOnShutdown(true);
        executor.initialize();   // 必须手动 initialize，才会真正创建底层 ExecutorService
        return executor;
    }
}
```

#### 关键属性解释

| 属性                                        | 含义                                                         |
| ------------------------------------------- | ------------------------------------------------------------ |
| `destroyMethod = "shutdown"`                | Spring 容器销毁 Bean 时会自动调用 `ThreadPoolTaskExecutor.shutdown()` |
| `setWaitForTasksToCompleteOnShutdown(true)` | 关闭时会 **等待已提交任务执行完**（等同于 `awaitTermination`） |
| `setAwaitTerminationSeconds(...)`           | 等待的最大秒数，超时后会强制 `shutdownNow()`                 |
| `setThreadNamePrefix`                       | 便于日志追踪线程来源                                         |
| `setQueueCapacity`                          | 控制任务排队长度，防止 OOM（队列满时会抛 `RejectedExecutionException`） |

### 2.2 在 Controller 中注入并使用

```java
@RestController
@RequestMapping("/api")
public class AsyncController {

    private final ThreadPoolTaskExecutor executor;

    public AsyncController(@Qualifier("appExecutor") ThreadPoolTaskExecutor executor) {
        this.executor = executor;
    }

    @PostMapping("/run")
    public ResponseEntity<String> runTask(@RequestBody SomePayload payload) {
        executor.submit(() -> {
            try {
                // 业务代码
                doHeavyJob(payload);
                log.info("Finished async job for {}", payload.getId());
            } catch (Exception e) {
                log.error("Error processing async job", e);
            }
        });
        return ResponseEntity.ok("Task submitted");
    }

    private void doHeavyJob(SomePayload payload) throws InterruptedException {
        // simulate work
        Thread.sleep(3000);
    }
}
```

> **优点**
>
> * Spring 自动管理 Bean 的 **生命周期** → 不必手写 `ServletContextListener`；
> * `@PreDestroy` 与 `DisposableBean` 同样可以实现自定义关闭逻辑；
> * 与 Spring `@Async`、`TaskScheduler` 等功能兼容，写法更统一。

### 2.3 进一步：使用 Spring 的 `@Async`

如果只是想把方法异步化，根本不需要显式拿到 `Executor`：

```java
@EnableAsync
@Configuration
public class AsyncConfig {
    @Bean(name = "taskExecutor")
    public Executor taskExecutor() {
        ThreadPoolTaskExecutor exec = new ThreadPoolTaskExecutor();
        exec.setCorePoolSize(4);
        exec.setMaxPoolSize(20);
        exec.setQueueCapacity(100);
        exec.setThreadNamePrefix("async-");
        exec.initialize();
        return exec;
    }
}

@Service
public class HeavyService {

    @Async("taskExecutor")    // 指定使用哪个线程池
    public CompletableFuture<Void> process(SomePayload payload) {
        // 业务代码
        return CompletableFuture.runAsync(() -> {
            // ...
        });
    }
}
```

Spring 会在容器关闭时自动调用 `Executor.shutdown()`（同样遵循 `awaitTermination` 设置），**无需手动销毁**。

---

## 3️⃣ 其他可选方案（Java EE / Jakarta EE）

如果你的项目运行在完整的 **Java EE** 容器（如 WildFly、WebSphere、Payara），可以直接使用 **容器托管的执行器**：

```java
@Resource
private ManagedExecutorService managedExecutor;
```

`ManagedExecutorService` 由容器生命周期管理，**容器会在应用停止时自动关闭**，不需要自行销毁。

> **优点**：符合 Java EE 标准，避免自行创建线程导致的资源泄漏。
> **缺点**：不是所有轻量容器（Tomcat、Jetty）都提供该 API，需要完整的 Java EE 服务器。

---

## 4️⃣ “合理销毁线程池” 的 **最佳实践 Checklist**

| 步骤                              | 说明                                                         | 代码片段                                                     |
| --------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **① 创建**                        | 用 **统一的入口**（Listener、Spring Bean、ManagedExecutor）创建，**一次**、**全局**。 | `executors = Executors.newFixedThreadPool(...);`             |
| **② 暴露**                        | 将实例放入 **ServletContext**、Spring **IoC 容器**，或使用 **JNDI** 进行检索。 | `ctx.setAttribute("APP_EXECUTOR", executor);`                |
| **③ 使用**                        | **仅提交任务** (`submit` / `execute`)；**不要在业务代码里 `shutdown`**。 | `executor.submit(runnable);`                                 |
| **④ 捕获异常**                    | 业务任务内部捕获 `Throwable`，防止线程意外退出导致线程池失效。 | `try { … } catch (Throwable t) { log.error(..., t); }`       |
| **⑤ 关闭入口**                    | 在容器 **关闭** 时：① `shutdown()` ② `awaitTermination(timeout)` ③ `shutdownNow()`（超时） ④ `removeAttribute` （防泄漏） | `listener.contextDestroyed` 代码                             |
| **⑥ 处理中断**                    | 若任务需要响应中断，检查 `Thread.currentThread().isInterrupted()` 或捕获 `InterruptedException` 并恢复中断状态。 | `catch (InterruptedException e) { Thread.currentThread().interrupt(); }` |
| **⑦ 日志 & 监控**                 | 记录 **线程池创建、关闭、异常、未完成任务数**，并结合 JMX/Prometheus 监控 `activeCount`、`queueSize`。 | `log.info("Pool stats: {}", executor);`                      |
| **⑧ 防止 ClassLoader 泄漏**       | 在 `contextDestroyed` 必须 **清除所有引用**（ServletContext、静态字段、线程本地变量 `ThreadLocal`）。 | `ctx.removeAttribute(...);`                                  |
| **⑨ 保持 Daemon/Non‑Daemon 正确** | **非 daemon**（默认）线程池让容器可以主动 shutdown；若用 **daemon**，容器关闭时可能不等任务完成，导致不完整的业务。 | `new ThreadFactoryBuilder().setDaemon(false)...`             |
| **⑩ 设定合理大小**                | 根据业务并发量、CPU、IO 特性，避免 **无限增长** 的 `CachedThreadPool` 导致 OOM。 | `newFixedThreadPool(core)`                                   |

---

## 5️⃣ 小结

| 场景                          | 推荐实现                                                     |
| ----------------------------- | ------------------------------------------------------------ |
| **纯 servlet 项目**           | `ServletContextListener` + `ExecutorService` 放入 `ServletContext`，在 `contextDestroyed` 中 **先 `shutdown` → `awaitTermination` → `shutdownNow`** |
| **Spring / Spring Boot 项目** | `ThreadPoolTaskExecutor`（或 `ExecutorService`）声明为 **Bean**，使用 `destroyMethod="shutdown"` 或 `@PreDestroy`。可配合 `@Async` 简化业务代码 |
| **Java EE / Jakarta EE**      | 注入 `ManagedExecutorService`，容器自行管理生命周期          |
| **需要高级调度**              | `ScheduledThreadPoolExecutor` + `@EnableScheduling`（Spring） 或 `Quartz`，同样遵循上面的 **统一销毁** 原则 |

只要 **创建、使用、销毁** 三个环节在同一个生命周期管理器（Listener / Spring Bean）里完成，就能保证线程池在 **应用启动时** 正确初始化，**应用关闭时** 能够 **优雅、可靠地释放**，避免内存泄漏、类加载器泄漏以及并发错误。