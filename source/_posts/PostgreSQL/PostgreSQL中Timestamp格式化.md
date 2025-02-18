---
title: PostgreSQL中Timestamp格式化
date: 2025-02-18 09:04:00
categories:
  - PostgreSQL
tags:
  - PostgreSQL
---

PostgreSQL 提供了多种方式来格式化 `timestamp` 或 `date` 类型的数据，以显示年月日时分秒。  常用的函数包括 `to_char()` 和 `format()`。

**1. 使用 `to_char()` 函数：**

`to_char(timestamp, format)` 函数将 `timestamp` 类型的值转换为文本字符串，并根据指定的 `format` 进行格式化。

示例：

```sql
SELECT to_char(NOW(), 'YYYY-MM-DD HH24:MI:SS');  -- 当前时间，24小时制
SELECT to_char(NOW(), 'YYYY-MM-DD HH12:MI:SS AM'); -- 当前时间，12小时制带 AM/PM

-- 示例数据
SELECT to_char(timestamp '2023-10-27 10:30:45', 'YYYY-MM-DD HH24:MI:SS'); -- 结果: 2023-10-27 10:30:45
SELECT to_char(date '2023-10-27', 'YYYY-MM-DD'); -- 结果: 2023-10-27 (只显示日期)

-- 从表中获取数据
SELECT to_char(creation_date, 'YYYY-MM-DD HH24:MI:SS') AS formatted_date
FROM my_table;
```

常用的格式化模式：

*   `YYYY`:  四位数年份
*   `YYY`, `YY`, `Y`: 年份 (不同位数)
*   `MM`:  两位数月份 (01-12)
*   `MON`:  月份缩写 (例如 Jan, Feb)
*   `MONTH`:  月份全称 (例如 January, February)
*   `DD`:  两位数日期 (01-31)
*   `DAY`:  星期几全称 (例如 Monday, Tuesday)
*   `HH24`:  24小时制小时 (00-23)
*   `HH12`:  12小时制小时 (01-12)
*   `MI`:  分钟 (00-59)
*   `SS`:  秒 (00-59)
*   `MS`:  毫秒 (000-999)
*   `US`:  微秒 (000000-999999)
*   `AM` 或 `PM`:  上下午标识

**2. 使用 `format()` 函数 (PostgreSQL 9.1 及更高版本)：**

`format(format_string, argument1, argument2, ...)` 函数提供了一种更灵活的格式化方式，它使用类似于 C 语言 `printf` 的格式化字符串。

示例：

```sql
SELECT format('%s %s', 'Hello', 'World');  -- 结果: Hello World

SELECT format('%1$s %2$s, %2$s %1$s', 'Hello', 'World'); -- 使用参数位置 (结果: Hello World, World Hello)

SELECT format('当前时间: %s', NOW()::text); -- 转换为 text 类型，否则报错

-- 格式化日期时间
SELECT format('日期: %1$s, 时间: %2$s', to_char(NOW(), 'YYYY-MM-DD'), to_char(NOW(), 'HH24:MI:SS'));

--更简洁的写法
SELECT format('%s', to_char(NOW(), 'YYYY-MM-DD HH24:MI:SS'));
```

`format()` 函数在处理日期时间时，通常需要结合 `to_char()` 函数将 `timestamp` 转换为字符串，然后再进行格式化。 直接使用 `format('%s', NOW())` 会报错，因为 `NOW()` 返回的是 `timestamp` 类型，`format` 函数需要字符串作为参数。 使用 `NOW()::text` 也是一种转换方式，但不如 `to_char` 灵活。

**选择哪个函数？**

*   **`to_char()`**:  更适合直接格式化日期和时间，尤其是当你只需要标准的日期时间格式时。它的性能通常也更好。
*   **`format()`**:  更适合复杂的字符串格式化，例如组合多个值或使用参数位置。  在处理日期时间时，需要结合 `to_char()`。

**重要提示：**

*   **类型转换：** 确保你的数据类型是 `timestamp` 或 `date`。如果你的数据是其他类型，你需要先进行类型转换，例如使用 `CAST(your_column AS timestamp)` 或 `your_column::timestamp`。
*   **时区：**  `NOW()` 函数返回的是服务器的当前时间。  如果你需要处理不同时区的时间，可以使用 `AT TIME ZONE` 子句。
*   **格式化模式：**  仔细阅读 PostgreSQL 文档，了解所有可用的格式化模式。

总结：

使用 `to_char` 函数通常是格式化 PostgreSQL 日期时间最简单和最有效的方法。 但是，`format` 函数在需要更复杂字符串操作时非常有用，但需要和 `to_char` 结合使用。