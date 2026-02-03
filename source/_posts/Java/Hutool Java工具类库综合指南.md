---
title: Hutool Java工具类库综合指南
date: 2026-02-03 09:42:11
categories:
  - Java
tags:
  - Java
  - Hutool
---
# Hutool Java工具类库综合指南

Hutool 是一个小而全的Java工具类库，通过静态方法封装，降低相关API的学习成本，提高工作效率，使Java拥有函数式语言般的优雅。

---

## 一、核心常用工具类

### 1. 字符串工具 - `StrUtil`

包含许多用于字符串操作的方法，如截取、格式化、拼接、判断空字符串等。

**功能示例：**
```java
// 判空操作
boolean empty = StrUtil.isEmpty(str);
boolean blank = StrUtil.isBlank(str);

// 格式化
String formatted = StrUtil.format("Hello, {}", "World");

// 截取
String subStr = StrUtil.sub(str, 0, 5);

// 转换
String camelCase = StrUtil.toCamelCase("under_score");
String underline = StrUtil.toUnderlineCase("camelCase");

// 填充
String padded = StrUtil.padPre("abc", 5, '0'); // "00abc"
```

### 2. 日期时间工具 - `DateUtil`

提供了日期和时间的解析、格式化、计算以及各种日期之间的转换。

**功能示例：**
```java
// 日期转换
Date date = DateUtil.parse("2023-05-01");

// 日期格式化
String formatted = DateUtil.format(new Date(), "yyyy-MM-dd HH:mm:ss");

// 日期计算
Date futureDate = DateUtil.offsetDay(new Date(), 3); // 3天后
long daysBetween = DateUtil.between(date1, date2, DateUnit.DAY); // 计算日期间隔
```

### 3. 文件IO工具 - `FileUtil`

提供了与文件系统交互的功能，包括创建、删除、移动文件，读写文件内容等。

**功能示例：**
```java
// 文件操作
FileUtil.touch("/path/to/file"); // 创建文件
FileUtil.mkdir("/path/to/dir"); // 创建目录
FileUtil.copy("/src", "/dest", true); // 复制文件/目录

// 文件读写
FileUtil.writeString("content", "/path/to/file", "UTF-8");
String content = FileUtil.readString("/path/to/file", "UTF-8");
```

### 4. 集合工具 - `CollUtil`

提供了对 `List`, `Set`, `Map` 等集合的操作方法，支持创建、添加或移除元素、转换集合等操作。

**功能示例：**
```java
// 创建集合
List<String> list = CollUtil.newArrayList("a", "b", "c");
Set<String> set = CollUtil.newHashSet("Apple", "Banana", "Orange");
Map<String, Integer> map = CollUtil.newHashMap("key1", 1, "key2", 2);

// 集合操作
boolean isEmpty = CollUtil.isEmpty(set);
CollUtil.sortByProperty(list, "age"); // 按属性排序
CollUtil.groupByField(list, "gender"); // 按字段分组
```

### 5. 类型转换工具 - `Convert`

提供了基础类型转换和特殊转换功能。

**功能示例：**
```java
// 基础类型转换
int i = Convert.toInt("123");
Date date = Convert.toDate("2023-05-01");

// 特殊转换
String unicode = Convert.strToUnicode("你好");
String hex = Convert.toHex("Hello".getBytes());
```

### 6. 加密解密工具 - `SecureUtil`

提供了多种加密算法的支持，如 MD5, SHA, AES 等。

**功能示例：**
```java
// 摘要算法
String md5 = SecureUtil.md5("password");
String sha256 = SecureUtil.sha256("data");

// 对称加密
AES aes = SecureUtil.aes(key);
String encrypted = aes.encryptHex("plaintext");
```

---

## 二、进阶实用工具类

### 7. HTTP客户端工具 - `HttpUtil`

简化了 HTTP 请求的发起，支持 GET, POST, PUT, DELETE 等请求方式。

**功能示例：**
```java
// GET请求
String result = HttpUtil.get("https://example.com");

// POST表单
Map<String, Object> params = new HashMap<>();
params.put("key1", "value1");
String response = HttpUtil.post("https://example.com", params);

// 文件下载
HttpUtil.downloadFile("https://example.com/file.zip", "/local/path/");
```

### 8. JSON操作工具类 - `JsonUtil`

支持将对象序列化为 JSON 字符串，以及将 JSON 字符串反序列化为对象。

**功能示例：**
```java
// 对象转 JSON 字符串
String jsonString = JsonUtil.toJson(object);

// JSON 字符串转对象
MyObject obj = JsonUtil.toBean(jsonString, MyObject.class);
```

### 9. 唯一ID生成工具 - `IdUtil`

**功能示例：**
```java
String uuid = IdUtil.randomUUID(); // UUID
long snowflakeId = IdUtil.getSnowflake().nextId(); // 雪花ID
String objectId = IdUtil.objectId(); // MongoDB风格ID
```

### 10. 二维码工具 - `QrCodeUtil`

**功能示例：**
```java
// 生成二维码到文件
QrCodeUtil.generate("https://hutool.cn", 300, 300, FileUtil.file("/path/qrcode.jpg"));

// 解析二维码
String content = QrCodeUtil.decode(FileUtil.file("/path/qrcode.jpg"));
```

### 11. Excel操作工具 - `ExcelUtil`

**功能示例：**
```java
// 读取Excel
List<User> users = ExcelUtil.getReader("/path/users.xlsx").readAll(User.class);

// 写入Excel
ExcelWriter writer = ExcelUtil.getWriter("/path/output.xlsx");
writer.write(users, true);
writer.close();
```

---

## 三、特色专业工具类

### 12. 缓存工具 - `CacheUtil`

**功能示例：**
```java
TimedCache<String, String> cache = CacheUtil.newTimedCache(1000 * 60);
cache.put("key", "value", 1000 * 30); // 30秒过期
String value = cache.get("key");
```

### 13. 定时任务工具 - `CronUtil`

**功能示例：**
```java
CronUtil.schedule("0 0/5 * * * ?", (Runnable) () -> {
    System.out.println("每5分钟执行一次");
});
CronUtil.start();
```

### 14. 验证码工具 - `CaptchaUtil`

**功能示例：**
```java
// 生成图形验证码
LineCaptcha captcha = CaptchaUtil.createLineCaptcha(200, 100);
captcha.write("/path/captcha.png");

// 验证码校验
boolean valid = captcha.verify("userInput");
```

### 15. 系统信息工具 - `OshiUtil` / `SystemUtil`

**功能示例：**
```java
// OshiUtil - 获取详细系统信息
SystemInfo si = OshiUtil.getSystemInfo();
System.out.println("CPU使用率: " + si.getHardware().getProcessor().getSystemCpuLoad());
System.out.println("内存总量: " + si.getHardware().getMemory().getTotal());

// SystemUtil - 获取基本系统信息
String osName = SystemUtil.get("os.name");
String jvmVersion = SystemUtil.getJavaVersion();
```

### 16. 输入输出流操作工具类 - `IoUtil`

为文件读写、复制、关闭流等操作提供便捷方法。

**功能示例：**
```java
// 关闭输入流
IoUtil.close(inputStream);

// 复制文件
File destFile = new File("dest/path/to/file.txt");
IoUtil.copy(new FileInputStream(sourceFile), new FileOutputStream(destFile));
```

### 17. 属性文件读取工具类 - `Prop`

方便地读取 `.properties` 文件中的配置项。

**功能示例：**
```java
Prop prop = new Prop("config.properties");
String value = prop.getStr("key");
```

### 18. Bean操作工具类 - `BeanUtil`

提供了 Bean 属性拷贝、获取和设置属性值等功能。

**功能示例：**
```java
// 拷贝属性值从 source 到 target
BeanUtil.copyProperties(sourceBean, targetBean);
```

### 19. 随机数生成工具类 - `RandomUtil`

用于生成随机字符串、数字等。

**功能示例：**
```java
// 生成随机字符串
String randomStr = RandomUtil.randomString(10);

// 生成随机整数
int randomNumber = RandomUtil.randomInt(1, 100);
```

### 20. URL构建工具类 - `UrlBuilder`

用于构建和解析 URL 地址。

**功能示例：**
```java
UrlBuilder urlBuilder = UrlBuilder.of("https://example.com")
    .addPath("api")
    .addPath("data")
    .addParam("id", "123");

String url = urlBuilder.build();
```

### 21. 资源加载工具类 - `ResourceUtil`

从 ClassPath 或者其他位置加载资源文件。

**功能示例：**
```java
// 从 ClassPath 加载文件
InputStream resourceAsStream = ResourceUtil.getStream("classpath:config.properties");
```

### 22. 校验工具类 - `ValidatorUtil`

提供了一些常见的校验方法，比如邮箱地址、手机号码、身份证号等。

**功能示例：**
```java
// 校验邮箱格式
boolean isValidEmail = ValidatorUtil.isEmail("user@example.com");

// 校验手机号格式
boolean isValidMobile = ValidatorUtil.isMobile("13800138000");
```

### 23. 压缩解压工具类 - `ZipUtil`

支持 ZIP 和 GZIP 的压缩和解压缩操作。

**功能示例：**
```java
// 压缩文件夹
ZipUtil.zip("path/to/source/folder", "path/to/destination/archive.zip");

// 解压缩文件
ZipUtil.unzip("path/to/archive.zip", "path/to/destination/folder");
```

---

## 四、使用建议

1. **按需引入**：
```xml
<!-- 只引入所需模块 -->
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-http</artifactId>
    <version>5.8.12</version>
</dependency>
```

2. **避免滥用**：对于简单操作，优先使用JDK原生方法

3. **性能敏感场景**：注意工具类的性能开销（如频繁IO操作）

4. **异常处理**：虽然工具类封装了异常，但关键操作仍需处理异常

---

## 五、最佳实践场景

| 场景 | 推荐工具类 | 优势 |
| ------------ | --------------------- | ---------------------- |
| REST API开发 | HttpUtil + JSONUtil | 简化HTTP请求和JSON处理 |
| 数据处理 | CollUtil + BeanUtil | 简化集合操作和对象转换 |
| 文件操作 | FileUtil + IoUtil | 简化文件读写和流操作 |
| 定时任务 | CronUtil + ThreadUtil | 轻量级定时任务管理 |
| 系统监控 | OshiUtil + SystemUtil | 获取系统运行时信息 |

> Hutool通过封装常用操作，使Java开发更高效优雅。但需注意：**工具类不是万能的，理解底层原理更重要**。
