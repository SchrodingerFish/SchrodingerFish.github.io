---
title: Hutool 中一些常用工具类的功能和使用方法
date: 2025-08-21 10:55:11
categories:
  - Java
tags:
  - Java
  - Hutool
---
## Hutool 中一些常用工具类的功能和使用方法


#### 1. CollUtil - 集合操作工具类

**功能：**

*   提供了对 `List`, `Set`, `Map` 等集合的操作方法。
*   支持创建、添加或移除元素、转换集合等操作。

**例子：**

```java
// 创建并初始化一个 Set
Set<String> set = CollUtil.newHashSet("Apple", "Banana", "Orange");

// 判断集合是否为空
boolean isEmpty = CollUtil.isEmpty(set);

// 将 List 转换为 Set，去重
Set<String> uniqueElements = CollUtil.newHashSet(list);
```

#### 2. StrUtil - 字符串处理工具类

**功能：**

*   包含了许多用于字符串操作的方法，如截取、格式化、拼接、判断空字符串等。

**例子：**

```java
// 检查字符串是否为空（null 或者空白）
boolean isBlank = StrUtil.isBlank("   ");

// 格式化字符串
String formatted = StrUtil.format("Hello, {}", "World");
```

#### 3. DateUtil - 日期时间处理工具类

**功能：**

*   提供了日期和时间的解析、格式化、计算以及各种日期之间的转换。

**例子：**

```java
// 获取当前时间
Date now = DateUtil.date();

// 格式化日期为指定格式的字符串
String formattedDate = DateUtil.format(now, "yyyy-MM-dd HH:mm:ss");

// 计算两个日期之间的差值
long daysBetween = DateUtil.between(date1, date2, DateUnit.DAY);
```

#### 4. IoUtil - 输入输出流操作工具类

**功能：**

*   为文件读写、复制、关闭流等操作提供便捷方法。

**例子：**

```java
// 关闭输入流
IoUtil.close(inputStream);

// 复制文件
File destFile = new File("dest/path/to/file.txt");
IoUtil.copy(new FileInputStream(sourceFile), new FileOutputStream(destFile));
```

#### 5. HttpUtil - HTTP 请求工具类

**功能：**

*   简化了 HTTP 请求的发起，支持 GET, POST, PUT, DELETE 等请求方式，并且可以轻松处理响应。

**例子：**

```java
// 发起 GET 请求
String response = HttpUtil.get("http://example.com/api/data");

// 发起 POST 请求
String postResponse = HttpUtil.post("http://example.com/api/post", "param1=value1&param2=value2");
```

#### 6. JsonUtil - JSON 操作工具类

**功能：**

*   支持将对象序列化为 JSON 字符串，以及将 JSON 字符串反序列化为对象。

**例子：**

```java
// 对象转 JSON 字符串
String jsonString = JsonUtil.toJson(object);

// JSON 字符串转对象
MyObject obj = JsonUtil.toBean(jsonString, MyObject.class);
```

#### 7. Prop - 属性文件读取工具类

**功能：**

*   方便地读取 `.properties` 文件中的配置项。

**例子：**

```java
Prop prop = new Prop("config.properties");
String value = prop.getStr("key");
```

#### 8. BeanUtil - Bean 操作工具类

**功能：**

*   提供了 Bean 属性拷贝、获取和设置属性值等功能。

**例子：**

```java
// 拷贝属性值从 source 到 target
BeanUtil.copyProperties(sourceBean, targetBean);
```

#### 9. CryptoUtil - 加密解密工具类

**功能：**

*   提供了多种加密算法的支持，如 MD5, SHA, AES 等。

**例子：**

```java
// 使用 MD5 加密
String md5 = DigestUtil.md5Hex("hello world");

// AES 加密解密
String encrypted = EncryptUtil.encryptAES("plaintext", "aesKey");
String decrypted = EncryptUtil.decryptAES(encrypted, "aesKey");
```

#### 10. RandomUtil - 随机数生成工具类

**功能：**

*   用于生成随机字符串、数字等。

**例子：**

```java
// 生成随机字符串
String randomStr = RandomUtil.randomString(10);

// 生成随机整数
int randomNumber = RandomUtil.randomInt(1, 100);
```

#### 11. FileUtil - 文件操作工具类

**功能：**

*   提供了与文件系统交互的功能，包括创建、删除、移动文件，读写文件内容等。

**例子：**

```java
// 读取文件内容为字符串
String content = FileUtil.readString(file, Charset.defaultCharset());

// 写入字符串到文件
FileUtil.writeString(content, file, Charset.defaultCharset());
```

#### 12. SystemUtil - 系统信息工具类

**功能：**

*   获取操作系统信息、环境变量、JVM 信息等。

**例子：**

```java
// 获取操作系统名称
String osName = SystemUtil.get("os.name");

// 获取 JVM 版本
String jvmVersion = SystemUtil.getJavaVersion();
```

#### 13. UrlBuilder - URL 构建工具类

**功能：**

*   用于构建和解析 URL 地址。

**例子：**

```java
UrlBuilder urlBuilder = UrlBuilder.of("https://example.com")
    .addPath("api")
    .addPath("data")
    .addParam("id", "123");

String url = urlBuilder.build();
```

#### 14. ResourceUtil - 资源加载工具类

**功能：**

*   从 ClassPath 或者其他位置加载资源文件。

**例子：**

```java
// 从 ClassPath 加载文件
InputStream resourceAsStream = ResourceUtil.getStream("classpath:config.properties");
```

#### 15. ValidatorUtil - 校验工具类

**功能：**

*   提供了一些常见的校验方法，比如邮箱地址、手机号码、身份证号等。

**例子：**

```java
// 校验邮箱格式
boolean isValidEmail = ValidatorUtil.isEmail("user@example.com");

// 校验手机号格式
boolean isValidMobile = ValidatorUtil.isMobile("13800138000");
```

#### 16. ZipUtil - 压缩解压工具类

**功能：**

*   支持 ZIP 和 GZIP 的压缩和解压缩操作。

**例子：**

```java
// 压缩文件夹
ZipUtil.zip("path/to/source/folder", "path/to/destination/archive.zip");

// 解压缩文件
ZipUtil.unzip("path/to/archive.zip", "path/to/destination/folder");
```
