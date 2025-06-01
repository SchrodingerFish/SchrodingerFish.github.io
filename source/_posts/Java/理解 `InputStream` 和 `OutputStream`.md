---
title: 理解Java中的InputStream和OutputStream
date: 2025-06-01 10:40:11
categories:
  - Java
tags:
  - Java
  - InputStream
  - OutputStream
---

理解 `InputStream` 和 `OutputStream` 中的 "in" 和 "out" 的关键在于，你要把自己想象成你的 **Java 程序**。 数据相对于你的程序来说，要么是进入程序（输入），要么是从程序出去（输出）。

*   **InputStream (输入流): "in" 代表 "进入" (into the program)**

    *   `InputStream` 用于**读取数据**。 想象一下，你正在从文件中读取数据，或者从网络连接中接收数据。 这些数据都 **"进入"** 你的 Java 程序。
    *   `InputStream` 的子类包括：
        *   `FileInputStream`：从文件中读取数据。
        *   `ByteArrayInputStream`：从字节数组中读取数据。
        *   `ObjectInputStream`：从流中读取对象。
        *   `Socket.getInputStream()`: 从网络连接中读取数据.

    *   你可以把 `InputStream` 想象成一根管道，数据从外部 **流入** 你的程序。

*   **OutputStream (输出流): "out" 代表 "出去" (out of the program)**

    *   `OutputStream` 用于**写入数据**。 想象一下，你正在将数据写入文件，或者通过网络连接发送数据。 这些数据都从你的 Java 程序 **"出去"**。
    *   `OutputStream` 的子类包括：
        *   `FileOutputStream`：向文件中写入数据。
        *   `ByteArrayOutputStream`：向字节数组中写入数据。
        *   `ObjectOutputStream`：向流中写入对象。
        *   `Socket.getOutputStream()`: 向网络连接中写入数据。

    *   你可以把 `OutputStream` 想象成一根管道，数据从你的程序 **流出** 到外部。

**总结:**

| 流类型      | 方向          | 描述                                                                                                         |
| ----------- | ----------- | ------------------------------------------------------------------------------------------------------------ |
| `InputStream` | **进入**程序 | 用于从各种源（文件、网络、内存等）**读取**数据到你的程序中。                                                                               |
| `OutputStream`| **出去**程序 | 用于将数据从你的程序**写入**到各种目标（文件、网络、内存等）。                                                                               |

**举例说明:**

假设你要将一个文件中的内容复制到另一个文件中：

```java
try (InputStream in = new FileInputStream("input.txt");
     OutputStream out = new FileOutputStream("output.txt")) {

    byte[] buffer = new byte[1024];
    int bytesRead;
    while ((bytesRead = in.read(buffer)) != -1) {
        out.write(buffer, 0, bytesRead);
    }

} catch (IOException e) {
    e.printStackTrace();
}
```

*   `FileInputStream("input.txt")` 创建了一个 `InputStream`，用于从 "input.txt" 文件 **读取** 数据。  数据从文件 **进入** 你的程序。
*   `FileOutputStream("output.txt")` 创建了一个 `OutputStream`，用于将数据 **写入** 到 "output.txt" 文件。  数据从你的程序 **出去** 到文件。
*   `in.read(buffer)` 从输入流读取数据到 `buffer` 字节数组中。
*   `out.write(buffer, 0, bytesRead)` 将 `buffer` 中的数据写入到输出流中。

通过把自己代入 Java 程序，并考虑数据的流向（进入程序还是离开程序），可以更容易地理解 `InputStream` 和 `OutputStream` 的含义。