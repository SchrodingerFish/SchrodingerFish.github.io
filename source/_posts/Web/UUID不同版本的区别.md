---
title: UUID不同版本的区别
date: 2025-02-25 15:17:00
categories:
  - Web
tags:
  - UUID
---


UUID（Universally Unique Identifier，通用唯一标识符）有多种版本，每个版本设计目标和使用场景不同，RFC 4122 文档对这些版本的定义进行了详细说明。以下是 UUID v1/v3/v4/v5 的主要区别和用法：

---

## **1. UUID v1**
- **基于时间戳和节点（MAC 地址）**
- **特点**：
    - 包含一个唯一的时间戳（当前时间）和设备的 MAC 地址。
    - 保证了同一设备不会生成重复的 UUID（因为 MAC 地址是独一无二的）。
    - 会暴露生成 UUID 的时间信息和设备信息（MAC 地址），存在隐私问题。
- **主要参数**：
    - 时间戳、高时钟顺序、MAC 地址。
- **使用场景**：
    - 需要基于时间有序排列的 UUID。
    - 适合分布式系统，但需要注意隐私泄漏问题。
- **示例**：
  ```bash
  e4eaaaf2-3a3b-11e9-b210-d663bd873d93
  ```

---

## **2. UUID v3**
- **基于名称（MD5 哈希）**
- **特点**：
    - 使用 MD5（不可逆的哈希算法）根据命名空间（namespace）和名称计算生成 UUID。
    - 同一命名空间中的相同输入名称总会生成相同的 UUID，因此适合需要唯一标识但重复可控的场景。
    - 不随机，计算结果是确定性的。
- **主要参数**：
    - 一个固定的命名空间 UUID + 一个名称字符串。
- **使用场景**：
    - 需要对已知名称计算相同的 UUID，例如用户的电子邮件、用户名等。
- **示例**：
  ```bash
  3d813cbb-47fb-32ba-91df-831e1593ac29
  ```

- **命名空间测试**：
  PostgreSQL 示例：
  ```sql
  SELECT uuid_generate_v3('6ba7b810-9dad-11d1-80b4-00c04fd430c8', 'example@test.com');
  ```

---

## **3. UUID v4**
- **基于随机数**
- **特点**：
    - 完全随机，由随机数生成器生成 UUID。
    - 随机性很高，冲突概率极低（理论上16^32中选取一个）。
    - 不包含时间戳或任何系统计算信息。
- **主要参数**：
    - 随机数。
- **使用场景**：
    - 随机生成标识符，不需要能预测的生成规律。
    - 适合分布式系统的标识。
- **示例**：
  ```bash
  f47ac10b-58cc-4372-a567-0e02b2c3d479
  ```

- 常见场景：
  ```sql
  SELECT uuid_generate_v4();
  SELECT gen_random_uuid();
  ```

---

## **4. UUID v5**
- **基于名称（SHA-1 哈希）**
- **特点**：
    - 与 UUID v3 类似，不同的是 v5 使用更安全的哈希算法（SHA-1 而非 MD5）。
    - 同一命名空间中的相同输入名称总会生成相同的 UUID。
    - 不随机，但比 v3 更安全。
- **主要参数**：
    - 一个固定的命名空间 UUID + 一个名称字符串。
- **使用场景**：
    - 与 v3 相似。
    - 更安全的哈希计算 UUID，因此适合对 UUID 的计算安全性有较高需求的场景。
- **示例**：
  ```bash
  c4a760a8-dbcf-5f02-a0ff-e6ff720f4afc
  ```

- 常见场景：
  ```sql
  SELECT uuid_generate_v5('6ba7b810-9dad-11d1-80b4-00c04fd430c8', 'example@test.com');
  ```

---

## **对比总结**

| **UUID版本** | **生成方式**                 | **是否随机** | **特点**                                                                 | **适用场景**                                              |
|--------------|-----------------------------|--------------|--------------------------------------------------------------------------|----------------------------------------------------------|
| **v1**       | 基于时间戳和 MAC 地址        | 否           | 时间戳生成，包含 MAC 地址（可追踪）。隐私数据暴露风险较高。                     | 需要有序 UUID，但需防止暴露设备信息或时间。              |
| **v3**       | 基于名称（MD5）             | 否           | 确定性，输入固定则 UUID 不变；容易受到 MD5 的安全性限制影响。                   | 标识需要唯一但不随机的资源，如用户邮箱、用户名。          |
| **v4**       | 基于随机数                  | 是           | 完全基于随机数生成，冲突概率极低，但不包含可追踪信息或排序依据。                 | 大部分场景，如分布式系统中生成数据库主键或唯一标识符。    |
| **v5**       | 基于名称（SHA-1）           | 否           | 确定性，比 v3 更安全（但 SHA-1 已非绝对安全）。                              | 类似 v3，适合更高安全性需求的唯一资源标识。              |

---

### 实际推荐
1. **随机生成 UUID（v4）：**
   如果不介意顺序和确定性，且关注性能和冲突风险，`v4` 是多数场景的选择。

2. **基于时间顺序 UUID（v1）：**
   如果需要按时间排序，可以选择 UUID v1，但要注意覆盖隐私信息（MAC 地址）。

3. **基于名称 UUID（v3 或 v5）：**
   如果需要根据某个命名确定 UUID（如确保给定用户名永远生成同一个 UUID），使用 v3 或 v5，建议优先使用 `v5`（安全性稍高）。