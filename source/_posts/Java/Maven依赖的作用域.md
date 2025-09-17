---
title: Maven依赖的作用域
date: 2025-09-17 11:20:11
categories:
    - Java
tags:
    - Java
    - Maven
---
在 Maven 中，`scope`（作用域）用于控制依赖项的可见性和生命周期。它决定了依赖项在哪些阶段可用，以及是否需要将依赖项打包到最终的构建产物（比如 JAR 或 WAR 文件）中。  `provided` 只是其中一种作用域，它具有特定的含义。

**`provided` 作用域：**

* **含义:**  `provided` 作用域表示依赖项将由运行环境（例如，Servlet 容器、JDK）提供。  这意味着，在编译、测试时需要用到这个依赖项，但是运行时不需要。  因此，Maven 不会将 `provided` 作用域的依赖项打包到最终的 JAR 或 WAR 文件中。

* **适用场景:**  典型的应用场景是 Servlet API。  当你开发一个 Web 应用时，你需要使用 Servlet API 中的接口和类进行编译和测试。  但是，Servlet API 实际上由 Servlet 容器 (例如 Tomcat 或 Jetty) 提供。  因此，你可以将 Servlet API 的依赖项设置为 `provided` 作用域，告诉 Maven 在构建时不要包含 Servlet API，而是假设运行时环境会提供这些依赖项。

* **例子:**

  ```xml
  <dependency>
      <groupId>jakarta.servlet</groupId>
      <artifactId>jakarta.servlet-api</artifactId>
      <version>6.0.0</version>
      <scope>provided</scope>
  </dependency>
  ```

**其他的 Maven 作用域：**

除了 `provided` 之外，Maven 还定义了其他的几种作用域，各有不同的用途：

* **`compile` (默认作用域):**

    * **含义:**  `compile` 作用域表示依赖项在编译、测试和运行时都可用。Maven 会将 `compile` 作用域的依赖项打包到最终的构建产物中。
    * **适用场景:**  这是最常用的作用域，适用于大多数依赖项，比如项目需要的核心库、第三方工具包等。
    * **例子:**

      ```xml
      <dependency>
          <groupId>org.apache.commons</groupId>
          <artifactId>commons-lang3</artifactId>
          <version>3.12.0</version>
          <scope>compile</scope> <!-- 可以省略，因为它是默认作用域 -->
      </dependency>
      ```

* **`runtime`:**

    * **含义:**  `runtime` 作用域表示依赖项在编译时不需要，但在测试和运行时需要。Maven 会将 `runtime` 作用域的依赖项打包到最终的构建产物中。
    * **适用场景:**  驱动程序（比如数据库驱动）通常使用 `runtime` 作用域。  在编译时，你只需要访问数据库的 JDBC 接口，而不需要驱动的具体实现。  但在运行时，你需要具体的驱动程序来连接数据库。
    * **例子:**

      ```xml
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>8.0.33</version>
          <scope>runtime</scope>
      </dependency>
      ```

* **`test`:**

    * **含义:**  `test` 作用域表示依赖项只在测试时可用，在编译和运行时不需要。Maven 不会将 `test` 作用域的依赖项打包到最终的构建产物中。
    * **适用场景:**  测试框架（比如 JUnit、Mockito）通常使用 `test` 作用域。
    * **例子:**

      ```xml
      <dependency>
          <groupId>org.junit.jupiter</groupId>
          <artifactId>junit-jupiter-api</artifactId>
          <version>5.9.3</version>
          <scope>test</scope>
      </dependency>
      ```

* **`system`:**

    * **含义:** `system` 作用域类似于 `provided`，但你需要显式指定依赖项的路径。Maven 不会在仓库中查找 `system` 作用域的依赖项。  **不推荐使用 `system` 作用域，因为它不具备可移植性。**
    * **适用场景:**  很少使用。如果某个依赖项不在 Maven 仓库中，并且你只想使用本地文件系统中的副本，可以使用 `system` 作用域。
    * **例子:**

      ```xml
      <dependency>
          <groupId>com.example</groupId>
          <artifactId>local-library</artifactId>
          <version>1.0</version>
          <scope>system</scope>
          <systemPath>${basedir}/src/main/resources/local-library.jar</systemPath>
      </dependency>
      ```

* **`import`:**

    * **含义:** `import` 作用域只适用于 `<dependencyManagement>` 部分，用于导入其他 POM 文件中的 `<dependencyManagement>` 配置。它不会直接引入依赖项。
    * **适用场景:** 用于模块化项目，提取公共的依赖管理配置。

**总结:**

`scope` 是 Maven 中一个重要的概念，它用于控制依赖项的可见性和生命周期。  正确使用 `scope` 可以确保你的项目依赖项配置正确，避免出现不必要的依赖冲突或运行时错误。  记住：

* `compile`: 默认，所有阶段可用。
* `provided`: 编译和测试可用，运行时由环境提供。
* `runtime`: 编译不需要，测试和运行时需要。
* `test`: 只在测试阶段可用。
* `system`: 类似于 provided, 需要指定路径，不推荐使用.
* `import`: 只在 dependencyManagement  中用于导入依赖管理信息.
