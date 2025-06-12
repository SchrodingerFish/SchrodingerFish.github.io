---
title: 基于 Spring Boot，异步下载文件到Minio
date: 2025-06-12 16:55:28
categories:
  - Java
tags:
  - Spring Boot
  - Minio
---

**项目结构**

```
my-async-file-downloader/
├── src/main/java/
│   └── com/example/downloader/
│       ├── config/
│       │   └── MinioConfig.java  // MinIO 配置
│       ├── controller/
│       │   └── DownloadController.java //  REST 控制器
│       ├── service/
│       │   ├── DownloadService.java   //  下载服务接口
│       │   └── DownloadServiceImpl.java //  下载服务实现
│       ├── model/
│       │   └── DownloadTask.java  //  下载任务模型
│       └── util/
│           └── AsyncUtils.java    //  异步工具类
├── src/main/resources/
│   └── application.properties  // 配置文件
├── pom.xml  // Maven 依赖
```

**1. 依赖 (pom.xml)**

首先，在 `pom.xml` 文件中添加必要的依赖。

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- MinIO SDK -->
    <dependency>
        <groupId>io.minio</groupId>
        <artifactId>minio</artifactId>
        <version>8.5.7</version> <!-- 确保使用最新的稳定版本 -->
    </dependency>

    <!-- Spring AOP (可选，用于日志记录和异常处理) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-aop</artifactId>
    </dependency>

    <!-- Lombok (可选，用于简化代码) -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

**2. MinIO 配置 (MinioConfig.java)**

```java
package com.example.downloader.config;

import io.minio.MinioClient;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MinioConfig {

    @Value("${minio.endpoint}")
    private String endpoint;

    @Value("${minio.accessKey}")
    private String accessKey;

    @Value("${minio.secretKey}")
    private String secretKey;

    @Bean
    public MinioClient minioClient() {
        return MinioClient.builder()
                .endpoint(endpoint)
                .credentials(accessKey, secretKey)
                .build();
    }
}
```

在 `application.properties` 中配置 MinIO 连接信息：

```properties
minio.endpoint=http://your-minio-endpoint:9000
minio.accessKey=YOUR_ACCESS_KEY
minio.secretKey=YOUR_SECRET_KEY
minio.bucketName=your-bucket-name  # 默认的 Bucket 名称
```

**3. 下载任务模型 (DownloadTask.java)**

```java
package com.example.downloader.model;

import lombok.Data;

@Data
public class DownloadTask {
    private String fileUrl;  // 文件下载 URL
    private String bucketName; // MinIO Bucket 名称
    private String objectName; // MinIO 对象名称
}
```

**4. 异步工具类 (AsyncUtils.java)**

```java
package com.example.downloader.util;

import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Component;

import java.util.concurrent.CompletableFuture;

@Component
public class AsyncUtils {

    @Async("taskExecutor")  //  使用配置的 taskExecutor
    public CompletableFuture<Void> executeAsync(Runnable task) {
        return CompletableFuture.runAsync(task);
    }
}
```

**5. 下载服务接口 (DownloadService.java)**

```java
package com.example.downloader.service;

import com.example.downloader.model.DownloadTask;

public interface DownloadService {
    void downloadFileToMinio(DownloadTask downloadTask);
}
```

**6. 下载服务实现 (DownloadServiceImpl.java)**

```java
package com.example.downloader.service;

import io.minio.MinioClient;
import io.minio.PutObjectArgs;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

import java.io.BufferedInputStream;
import java.io.InputStream;
import java.net.URL;
import java.net.URLConnection;

import com.example.downloader.model.DownloadTask;

@Service
@Slf4j
@RequiredArgsConstructor
public class DownloadServiceImpl implements DownloadService {

    private final MinioClient minioClient;

    @Value("${minio.bucketName}")
    private String defaultBucketName;

    @Override
    public void downloadFileToMinio(DownloadTask downloadTask) {
        String fileUrl = downloadTask.getFileUrl();
        String bucketName = (downloadTask.getBucketName() != null && !downloadTask.getBucketName().isEmpty()) ? downloadTask.getBucketName() : defaultBucketName;
        String objectName = downloadTask.getObjectName();

        log.info("开始下载文件: URL={}, Bucket={}, Object={}", fileUrl, bucketName, objectName);

        try {
            URL url = new URL(fileUrl);
            URLConnection connection = url.openConnection();
            connection.setConnectTimeout(5000); // 设置连接超时
            connection.setReadTimeout(10000);  // 设置读取超时

            try (InputStream inputStream = new BufferedInputStream(connection.getInputStream())) {
                PutObjectArgs putObjectArgs = PutObjectArgs.builder()
                        .bucket(bucketName)
                        .object(objectName)
                        .stream(inputStream, connection.getContentLength(), -1)
                        .contentType(connection.getContentType())
                        .build();

                minioClient.putObject(putObjectArgs);
                log.info("文件 {} 下载并上传到 MinIO 成功: Bucket={}, Object={}", fileUrl, bucketName, objectName);

            } catch (Exception e) {
                log.error("上传文件到 MinIO 失败: URL={}, Bucket={}, Object={}", fileUrl, bucketName, objectName, e);
                throw new RuntimeException("上传文件到MinIO失败", e); // 或者抛出自定义异常
            }

        } catch (Exception e) {
            log.error("下载文件失败: URL={}", fileUrl, e);
            throw new RuntimeException("下载文件失败", e); // 或者抛出自定义异常
        }
    }
}
```

**7. REST 控制器 (DownloadController.java)**

```java
package com.example.downloader.controller;

import com.example.downloader.model.DownloadTask;
import com.example.downloader.service.DownloadService;
import com.example.downloader.util.AsyncUtils;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.concurrent.CompletableFuture;

@RestController
@RequestMapping("/download")
@Slf4j
@RequiredArgsConstructor
public class DownloadController {

    private final DownloadService downloadService;
    private final AsyncUtils asyncUtils;

    @PostMapping("/async")
    public ResponseEntity<String> downloadFileAsync(@RequestBody DownloadTask downloadTask) {
        log.info("收到异步下载请求: {}", downloadTask);

        // 异步执行下载任务
        CompletableFuture<Void> future = asyncUtils.executeAsync(() -> {
            try {
                downloadService.downloadFileToMinio(downloadTask);
            } catch (Exception e) {
                log.error("异步下载任务失败", e);
                //  考虑添加重试机制或错误处理逻辑
            }
        });

        //  可以添加 future.exceptionally() 来处理异步任务中的异常

        return ResponseEntity.status(HttpStatus.ACCEPTED).body("下载任务已提交，正在后台执行.");
    }
}
```

**8. 异步配置 (AsyncConfig.java - 可选)**

如果你需要自定义线程池，可以创建一个配置类：

```java
package com.example.downloader.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

import java.util.concurrent.Executor;

@Configuration
@EnableAsync
public class AsyncConfig {

    @Bean(name = "taskExecutor")
    public Executor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);  // 核心线程数
        executor.setMaxPoolSize(10); // 最大线程数
        executor.setQueueCapacity(25); // 队列大小
        executor.setThreadNamePrefix("Async-Download-");
        executor.initialize();
        return executor;
    }
}
```

**重要说明和最佳实践**

* **异常处理:**  代码中包含基本的异常处理，但在实际生产环境中，应该进行更完善的异常处理，例如：
    *   自定义异常类，以便更好地区分不同类型的错误。
    *   重试机制，如果下载或上传失败，可以尝试重新执行任务。
    *   监控和告警，当出现错误时，发送通知。
* **日志记录:** 使用 `slf4j` 记录详细的日志信息，方便调试和监控。
* **线程池配置:**  根据实际需求调整线程池的大小。  `corePoolSize` 设置核心线程数，`maxPoolSize` 设置最大线程数，`queueCapacity` 设置队列大小。 合理的配置可以提高并发性能。
* **MinIO Bucket 权限:** 确保 MinIO Bucket 具有正确的读写权限。
* **安全性:**
    *  不要在代码中硬编码 MinIO 密钥。 使用环境变量或配置文件来管理敏感信息。
    *  考虑使用 HTTPS 连接 MinIO。
* **文件命名:**  在 `DownloadTask` 中，`objectName`  需要根据你的业务逻辑生成唯一的文件名，避免冲突。
* **异步任务状态:** 如果需要跟踪异步任务的状态（例如，成功、失败、进度），可以考虑使用消息队列（例如 RabbitMQ, Kafka）或数据库来存储任务状态。
* **文件大小:** 下载大文件时，应该使用流式上传，避免将整个文件加载到内存中。  `MinioClient.putObject`  方法已经支持流式上传。
* **连接超时和读取超时:**  设置合理的连接超时和读取超时时间，防止程序长时间阻塞。
* **幂等性:** 考虑实现幂等性，即多次执行相同的下载任务应该产生相同的结果。 这可以通过检查 MinIO 中是否已存在同名文件来实现。
* **测试:** 编写单元测试和集成测试，确保代码的正确性。
* **监控和告警:** 使用监控工具（例如 Prometheus, Grafana）监控应用程序的性能和健康状况，并在出现问题时发送告警。
* **Spring Boot Actuator:**  使用 Spring Boot Actuator 来监控应用程序的健康状况和性能指标。
* **资源管理:** 确保在使用完 `InputStream` 后关闭它，避免资源泄漏。 使用 `try-with-resources` 语句可以自动关闭资源。

**如何运行**

1.  克隆或下载项目代码。
2.  配置 `application.properties` 文件，设置 MinIO 连接信息和 Bucket 名称。
3.  使用 Maven 构建项目： `mvn clean install`
4.  运行 Spring Boot 应用程序。

**如何测试**

使用 Postman 或 curl 发送 POST 请求到 `/download/async` 接口，请求体为 JSON 格式的 `DownloadTask` 对象。 例如：

```json
{
  "fileUrl": "https://www.example.com/example.pdf",
  "bucketName": "my-bucket",
  "objectName": "example.pdf"
}
```

服务器将返回 `202 Accepted` 状态码，表示下载任务已提交到后台执行。  你可以登录 MinIO 控制台查看文件是否已成功上传。

这个设计方案提供了一个基本的异步文件下载到 MinIO 的 Spring Boot 项目。