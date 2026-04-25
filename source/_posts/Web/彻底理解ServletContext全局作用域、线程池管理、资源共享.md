---
title: 彻底理解 `ServletContext`：全局作用域、线程池管理、资源共享
date: 2026-04-25 08:23:11
categories:
  - Web
tags:
  - ServletContext
  - Java Servlet
---

# 彻底理解 `ServletContext`：全局作用域、线程池管理、资源共享

`ServletContext` 是 **Servlet 规范** 为每一个 Web 应用（即 **一个** `WAR` 包）在 **Servlet 容器** 中提供的 **全局运行时对象**。
它相当于 **“Web 应用的全局作用域”**，所有在同一个 `WebApplication`（同一个 `Context`、同一个类加载器）内部的 **Servlet、Filter、Listener、JSP、静态资源** 都可以共享它。

| 维度         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| **生命周期** | 从 **Web 应用启动（容器加载 `web.xml` / 注解）** 到 **Web 应用被卸载/服务器关闭**。 |
| **唯一实例** | 对于同一个 Web 应用，容器只会创建 **一个** `ServletContext` 实例。 |
| **全局可见** | 任何在该 Web 应用内部的组件（Servlet、Filter、Listener、JSP、静态类）都能通过不同方式取得同一个实例。 |
| **线程安全** | `ServletContext` 本身是 **线程安全** 的，容器保证对它的方法（比如 `setAttribute/getAttribute`）是同步的。<br>但是放进去的对象 **自行决定** 是否线程安全——如果是 `Map、List` 等可变对象，需要自行做好同步或使用并发集合。 |

---

## 1️⃣ ServletContext 能干什么？

| 功能                                                         | 示例                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **获取初始化参数**（在 `web.xml` 或注解中配置的 `<context-param>`） | `String dbUrl = ctx.getInitParameter("jdbcUrl");`            |
| **读取 Web 资源**（文件、图片、配置文件等）                  | `InputStream is = ctx.getResourceAsStream("/WEB-INF/config.properties");` |
| **注册/获取全局属性**（跨 Servlet/Filter/Listener 共享的数据） | `ctx.setAttribute("myPool", executor);`<br>`ExecutorService pool = (ExecutorService) ctx.getAttribute("myPool");` |
| **获取路径信息**                                             | `String realPath = ctx.getRealPath("/WEB-INF");`（在某些容器里可能返回 `null`，因为资源可能在压缩包内） |
| **日志**                                                     | `ServletContext.log("业务启动");`（等价于 `System.out.println`，但会写入容器日志） |
| **获取 MIME 类型**                                           | `String mime = ctx.getMimeType("image.png");`                |
| **发布/监听属性变更事件**                                    | `ctx.addAttributeListener(new MyAttrListener());`            |
| **获取 Servlet API 版本信息**                                | `ctx.getMajorVersion(); ctx.getMinorVersion();`              |

> **核心思想**：把 **“全局、一次性、在整个 Web 应用周期内有效”** 的东西放进 `ServletContext`，这样所有组件都能统一访问，而不必每次重新创建。

---

## 2️⃣ 如何取得 ServletContext？

| 场景                                                         | 取得方式                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **在 Servlet、Filter、JSP**                                  | `ServletContext ctx = getServletContext();` （Servlet、Filter 里直接继承）<br>`ServletContext ctx = request.getServletContext();`（在 `HttpServletRequest` 中） |
| **在 Listener**（`ServletContextListener`、`HttpSessionListener`、`ServletRequestListener`） | `ServletContext ctx = sce.getServletContext();`（`ServletContextEvent` 参数） |
| **在普通 Java 类**（没有直接继承 Servlet）                   | 1️⃣ **通过注入**（在 Spring、Guice 中）<br>2️⃣ **通过全局持有**：在 `ServletContextListener` 启动时把它保存到一个 `static` 变量（**慎用**，要在 `contextDestroyed` 时置 `null` 防止类加载器泄漏） |
| **在 JSP 页面**                                              | 隐式对象 `${application}` 或 `<%= application %>`（JSP 自带的 `ServletContext` 实例） |
| **在 Servlets 初始化时**                                     | `public void init()` 方法里可以直接使用 `getServletContext()` |

### 示例：在 Listener 中把线程池放进 ServletContext

```java
@WebListener
public class ThreadPoolListener implements ServletContextListener {

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        ExecutorService pool = Executors.newFixedThreadPool(8);
        sce.getServletContext().setAttribute("APP_EXECUTOR", pool);
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        ExecutorService pool = (ExecutorService) sce.getServletContext()
                                                   .getAttribute("APP_EXECUTOR");
        if (pool != null) {
            pool.shutdown();
        }
        sce.getServletContext().removeAttribute("APP_EXECUTOR"); // 防止泄漏
    }
}
```

---

## 3️⃣ 常见使用场景（Pure Servlet 项目）

下面列出几个实际开发中经常把对象放进 `ServletContext` 的 **典型场景**，并针对每种情况给出注意事项。

### 3.1 配置/字典数据（只读）

```xml
<!-- web.xml -->
<context-param>
    <param-name>appVersion</param-name>
    <param-value>1.5.2</param-value>
</context-param>
```

```java
public class VersionServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) 
                         throws ServletException, IOException {
        String version = getServletContext().getInitParameter("appVersion");
        resp.getWriter().write("Current version: " + version);
    }
}
```

* **为什么放这里**：配置信息只读、全局共享，省去每次读取 `properties` 文件的 I/O。

### 3.2 数据库连接池（可变但生命周期与应用相同）

```java
public class DBPoolListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent e) {
        DataSource ds = createDataSource();               // 参考 DBCP/HikariCP
        e.getServletContext().setAttribute("DB_POOL", ds);
    }

    @Override
    public void contextDestroyed(ServletContextEvent e) {
        DataSource ds = (DataSource) e.getServletContext()
                                        .getAttribute("DB_POOL");
        if (ds instanceof Closeable) {
            ((Closeable) ds).close(); // 关闭连接池
        }
        e.getServletContext().removeAttribute("DB_POOL");
    }
}
```

> **注意**：
>
> * 必须在 `contextDestroyed` 里显式 **关闭** 资源，否则会导致 **类加载器泄漏**（Web 应用在热部署时，旧的连接池线程仍在运行）。
> * 为防止业务代码在容器关闭后仍尝试使用池子，可在 `contextDestroyed` 里把 `DB_POOL` 置 `null`，并在业务层捕获 `NullPointerException` 或 `IllegalStateException`。

### 3.3 线程池（正是你之前的需求）

> 见前面「ServletContextListener」章节的完整实现。
> 把 `ExecutorService` 放进 `ServletContext`，在所有 Servlet/Filter 中通过 `getServletContext().getAttribute("APP_EXECUTOR")` 取得，再 `submit` 任务。关闭时遵循 **先 shutdown → 等待 → 强制 shutdownNow** 的顺序。

### 3.4 缓存（例如 Guava Cache、Ehcache）

```java
public class CacheListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent e) {
        Cache<String, Object> cache = CacheBuilder.newBuilder()
                .maximumSize(10_000)
                .expireAfterWrite(30, TimeUnit.MINUTES)
                .build();
        e.getServletContext().setAttribute("APP_CACHE", cache);
    }

    @Override
    public void contextDestroyed(ServletContextEvent e) {
        Cache<?,?> cache = (Cache<?,?>) e.getServletContext()
                                         .getAttribute("APP_CACHE");
        if (cache != null) {
            cache.cleanUp(); // 可选，确保内部线程关闭
        }
        e.getServletContext().removeAttribute("APP_CACHE");
    }
}
```

### 3.5 国际化资源（MessageBundle）

```java
ResourceBundle bundle = ResourceBundle.getBundle("messages", Locale.CHINA);
ctx.setAttribute("MSG_BUNDLE", bundle);
```

> **好处**：避免每次请求都重新加载 `ResourceBundle`（内部已经会缓存，但可以显式控制加载时机）。

---

## 4️⃣ 与 **HttpSession**、**ServletRequest** 的区别

| 对象               | 作用域                                     | 生命周期                                             | 取值方式                                                     | 常见用途                     |
| ------------------ | ------------------------------------------ | ---------------------------------------------------- | ------------------------------------------------------------ | ---------------------------- |
| **ServletContext** | **全局（Web 应用层）**                     | 从容器 **启动** 到 **卸载**                          | `getServletContext()`、`ServletContextEvent.getServletContext()` | 配置、资源、全局缓存、线程池 |
| **HttpSession**    | **单个用户会话**（基于 cookie/JSESSIONID） | 从 **首次访问** 到 **会话失效**（超时/手动 invalid） | `request.getSession()`                                       | 登录信息、购物车、用户偏好   |
| **ServletRequest** | **一次请求**（包括 forward/include）       | **单次 HTTP 请求** 结束即销毁                        | `request.getAttribute()`                                     | 请求级别的临时数据、转发参数 |

> **注意**：`ServletContext` 中的对象会被 **所有用户共享**，一定要保证线程安全。相反，`HttpSession` 是对每个会话独立的，即使对象不是线程安全也可以（因为同一时间只能有一个请求对同一个 Session 进行写操作，容器会对其加锁）。但如果同一用户打开多个页面并并发请求同一 Session，仍会出现并发访问，需要自行同步。

---

## 5️⃣ 使用 `ServletContext` 时的 **最佳实践**

| 关键点                                  | 说明                                                         | 示例                                                         |
| --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **只在容器启动时创建一次**              | 把耗时、资源占用大的对象放在 `ServletContextListener` 的 `contextInitialized` 中，避免在每个请求里重复创建 | `ExecutorService pool = Executors.newFixedThreadPool(...);`  |
| **在 `contextDestroyed` 完整销毁**      | 必须 **shutdown**、**close**、**removeAttribute**，防止类加载器泄漏 | `pool.shutdown(); ctx.removeAttribute("APP_EXECUTOR");`      |
| **属性名统一、避免冲突**                | 建议使用 **全大写、项目前缀**（如 `MYAPP_EXECUTOR`）         | `ctx.setAttribute("MYAPP_EXECUTOR", pool);`                  |
| **属性值要么不可变，要么是线程安全的**  | 放入 `Map`、`List` 时使用 `Collections.synchronizedMap` 或 `ConcurrentHashMap` | `new ConcurrentHashMap<>()`                                  |
| **不要在属性里放入 `ThreadLocal`**      | `ThreadLocal` 绑定到线程，线程池中的线程会一直保持引用，导致 **内存泄漏** | 只在业务代码里使用 `ThreadLocal`，完成后 `remove()`          |
| **尽量不要把 `Servlet` 实例本身放进去** | `Servlet` 实例由容器管理，自己持有会导致生命周期混乱         | **不要** `ctx.setAttribute("myServlet", this);`              |
| **日志统一**                            | 使用 `ServletContext.log(...)` 或容器日志框架，便于在容器日志文件中统一查看 | `ctx.log("Thread pool created");`                            |
| **利用 JMX 监控**                       | 如果使用 `ThreadPoolExecutor`，可以把它包装成 JMX MBean，放在 `ServletContext` 中供运维监控 | `ManagementFactory.getPlatformMBeanServer().registerMBean(...)` |
| **避免硬编码路径**                      | 读取资源时使用 `ctx.getResourceAsStream("/WEB-INF/…")`，而不是相对磁盘路径 | `InputStream is = ctx.getResourceAsStream("/WEB-INF/config.xml");` |

### 防止 **ClassLoader 泄漏**（热部署常见坑）

1. **在 `contextDestroyed`** 必须把 **所有放入 `ServletContext`** 的对象 **都清掉**（`removeAttribute`），并确保它们内部的线程已退出。
2. **如果使用 `static` 变量** 来缓存 `ServletContext`（比如 `public static ServletContext CONTEXT;`），在 `contextDestroyed` 必须 **设为 null**。
3. **避免把 `Thread`、`Timer`、`Executor`** 等 **长生命周期的线程** 直接存为 **静态成员**，而不交由容器管理。

---

## 6️⃣ 小实例：完整的“全局线程池 + Servlet 调用” 示例

> **目录结构**（pure servlet, no Spring）
>
> ```
> src/
> └─ main/
> └─ java/
>    ├─ listener/ThreadPoolListener.java
>    └─ servlet/AsyncTaskServlet.java
> └─ webapp/
>    └─ WEB-INF/web.xml   (optional, because我们使用 @WebListener/@WebServlet)
> ```

### 6.1 `ThreadPoolListener.java`

```java
package listener;

import jakarta.servlet.*;
import jakarta.servlet.annotation.WebListener;
import java.util.concurrent.*;
import java.util.List;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@WebListener
public class ThreadPoolListener implements ServletContextListener {

 private static final Logger log = LoggerFactory.getLogger(ThreadPoolListener.class);
 public static final String POOL_ATTR = "APP_EXECUTOR";

 private ExecutorService executor;

 @Override
 public void contextInitialized(ServletContextEvent sce) {
 int core = Runtime.getRuntime().availableProcessors() * 2;
 executor = Executors.newFixedThreadPool(core, r -> {
 Thread t = new Thread(r);
 t.setName("app-pool-" + t.getId());
 t.setDaemon(false); // 非 daemon，容器才能主动关闭
 return t;
 });
 sce.getServletContext().setAttribute(POOL_ATTR, executor);
 log.info("Thread pool created, coreSize={}", core);
 }

 @Override
 public void contextDestroyed(ServletContextEvent sce) {
 ExecutorService exec = (ExecutorService) sce.getServletContext()
 .getAttribute(POOL_ATTR);
 if (exec == null) {
 log.warn("Executor not found during destroy");
 return;
 }

 // ① 不再接受新任务
 exec.shutdown();
 try {
 // ② 等待正在执行的任务结束（30 秒）
 if (!exec.awaitTermination(30, TimeUnit.SECONDS)) {
 // ③ 超时 → 强制关闭
 List<Runnable> waiting = exec.shutdownNow();
 log.warn("Force shutdown, {} tasks were awaiting execution", waiting.size());
 // 再等几秒确保线程真正退出
 if (!exec.awaitTermination(10, TimeUnit.SECONDS)) {
 log.error("Thread pool still not terminated after forced shutdown");
 }
 }
 } catch (InterruptedException e) {
 exec.shutdownNow(); // 立即强制
 Thread.currentThread().interrupt(); // 恢复中断状态
 }

 // ④ 清理引用
 sce.getServletContext().removeAttribute(POOL_ATTR);
 log.info("Thread pool shut down cleanly");
 }
}
```

### 6.2 `AsyncTaskServlet.java`

```java
package servlet;

import jakarta.servlet.*;
import jakarta.servlet.http.*;
import jakarta.servlet.annotation.WebServlet;
import java.io.IOException;
import java.util.concurrent.ExecutorService;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@WebServlet("/async")
public class AsyncTaskServlet extends HttpServlet {

 private static final Logger log = LoggerFactory.getLogger(AsyncTaskServlet.class);
 private ExecutorService executor;

 @Override
 public void init() throws ServletException {
 // 在 init 里取得已经放入 ServletContext 的线程池
 ServletContext ctx = getServletContext();
 executor = (ExecutorService) ctx.getAttribute("APP_EXECUTOR");
 if (executor == null) {
 throw new ServletException("ExecutorService not found in ServletContext");
 }
 }

 @Override
 protected void doGet(HttpServletRequest req, HttpServletResponse resp)
 throws ServletException, IOException {

 // 这里演示把一个耗时任务交给线程池
 executor.submit(() -> {
 try {
 // 假设是业务耗时计算
 Thread.sleep(2000);
 log.info("Async job finished, thread={}", Thread.currentThread().getName());
 } catch (InterruptedException e) {
 Thread.currentThread().interrupt();
 log.warn("Async job interrupted");
 } catch (Exception e) {
 log.error("Exception in async job", e);
 }
 });

 resp.setContentType("text/plain;charset=UTF-8");
 resp.getWriter().write("Task has been submitted");
 }
}
```

**运行流程**：

1. **容器启动** → `ThreadPoolListener.contextInitialized` 被调用 → 创建 `ExecutorService` 并放进 `ServletContext`。
2. **请求** `/async` → `AsyncTaskServlet.init()` 读取同一个 `ExecutorService` → `doGet` 把任务 `submit` 到线程池。
3. **容器关闭** → `ThreadPoolListener.contextDestroyed` 负责优雅关闭线程池，防止残留线程导致内存泄漏。

---

## 7️⃣ 常见错误 & 解决方案

| 错误情形                                                     | 说明                                                         | 正确做法                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **在业务代码里直接 `executor.shutdown()`**                   | 关闭后后续请求全部失效。                                     | **只在 Listener**（容器关闭时）进行关闭。                    |
| **把 `ExecutorService` 放在 `static` 变量里且不在 `contextDestroyed` 中置 `null`** | 热部署（Tomcat reload）时旧的线程仍在运行 → 类加载器泄漏。   | 使用 `ServletContext`，并在 `contextDestroyed` 中 `removeAttribute`。 |
| **线程池内部的任务没有捕获 `Throwable`**                     | 未捕获异常会导致 **工作线程** 退出，线程池变少，后续提交会阻塞或抛 `RejectedExecutionException`。 | 在每个 `Runnable/Callable` 外层 `try/catch Throwable` 并记录日志。 |
| **把 `ThreadLocal` 放进全局对象**                            | 线程池线程不会自动清理 `ThreadLocal`，导致内存泄漏。         | 在任务结束时 `threadLocal.remove()`，或避免在全局对象里使用 `ThreadLocal`。 |
| **使用 `Executors.newCachedThreadPool()` 在高并发下导致 OOM** | 该实现会无限创建线程。                                       | 使用 `ThreadPoolExecutor`，设定 `corePoolSize`、`maximumPoolSize`、`queueCapacity`。 |
| **在 `contextDestroyed` 里只调用 `shutdown()`，不等待**      | 容器可能在线程仍在执行时直接停止，导致业务未完成。           | 使用 `awaitTermination` 并在超时后 `shutdownNow()`。         |
| **在 `ServletContextListener` 中抛异常**                     | 容器可能认为启动失败，导致应用不可用。                       | 捕获异常并记录日志，除非真的需要中止启动。                   |

---

## 8️⃣ 小结

| 关键点                                         | 解释                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ |
| **ServletContext 是整个 Web 应用的全局作用域** | 一个实例、生命周期覆盖容器启动/停止、所有组件共享。          |
| **创建一次、销毁一次**                         | 把资源（线程池、连接池、缓存等）放在 `ServletContextListener` 中创建，在 `contextDestroyed` 中统一关闭。 |
| **使用 `setAttribute/getAttribute` 进行共享**  | 只要键名唯一、值是线程安全的即可。                           |
| **销毁要优雅**                                 | `shutdown()` → `awaitTermination(timeout)` → `shutdownNow()` → `removeAttribute`，防止线程泄漏与类加载器泄漏。 |
| **永远不要在业务代码里自行 `shutdown`**        | 那会导致全局资源提前失效。                                   |
| **注意线程安全、异常捕获、`ThreadLocal` 清理** | 否则会出现并发错误或内存泄漏。                               |
| **若使用 Spring、Java EE**                     | 可以让容器/框架管理线程池（`ThreadPoolTaskExecutor`、`ManagedExecutorService`），但原理仍然是“在容器停机时关闭”。 |

