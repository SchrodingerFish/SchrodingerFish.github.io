---
title: PostgreSQL转义字符处理
date: 2025-04-02 09:00:18
categories:
  - PostgreSQL
tags:
  - PostgreSQL
---

`quote_literal` 函数是 PostgreSQL 中的一个内置函数，用于对字符串进行转义，使其能够安全地用作 SQL 语句中的字符串字面量。  它主要用于防止 SQL 注入攻击。

**功能:**

`quote_literal` 函数接收一个字符串作为输入，并返回一个经过转义处理的字符串。  转义处理包括：

*   将字符串中的单引号 (`'`) 替换为两个单引号 (`''`)。
*   如果 `standard_conforming_strings` 配置参数关闭（在 PostgreSQL 9.1 之前，默认为关闭），则还将反斜杠 (`\`) 替换为两个反斜杠 (`\\`)。

**语法:**

```sql
quote_literal(string text)
```

**参数:**

*   `string`: 要进行转义的字符串。

**返回值:**

*   转义后的字符串。

**示例:**

```sql
SELECT quote_literal('O''Reilly');  -- 返回 'O''Reilly'
SELECT quote_literal('C:\path\to\file'); -- 返回 'C:\path\to\file' (如果 standard_conforming_strings 为 on)
```

**用途:**

`quote_literal` 主要用于动态构建 SQL 查询，特别是当字符串值来自用户输入或其他不可信来源时。  通过使用 `quote_literal`，可以确保即使字符串包含单引号或其他特殊字符，也不会破坏 SQL 语句的语法，从而防止 SQL 注入攻击。

**为什么需要 `quote_literal`？**

考虑以下情况：你想要根据用户输入的名称查询数据库中的记录。  如果直接将用户输入的值插入到 SQL 查询中，可能会出现问题：

```sql
-- 危险的做法：直接将用户输入插入到 SQL 查询中
-- 假设用户输入的 name 是 "O'Reilly"
SELECT * FROM users WHERE name = 'O'Reilly';  -- 这会导致 SQL 语法错误！
```

上面的 SQL 语句会因为 `O'Reilly` 中的单引号而导致语法错误。  更糟糕的是，恶意用户可以输入包含恶意 SQL 代码的字符串，从而执行未经授权的操作。

使用 `quote_literal` 可以解决这个问题：

```sql
-- 安全的做法：使用 quote_literal 转义用户输入
-- 假设用户输入的 name 是 "O'Reilly"
SELECT * FROM users WHERE name = quote_literal('O''Reilly');  -- 转义后的 SQL 语句是 SELECT * FROM users WHERE name = 'O''Reilly';

-- 或者，使用更安全的参数化查询（推荐）：
PREPARE myquery (text) AS
    SELECT * FROM users WHERE name = $1;
EXECUTE myquery('O''Reilly');
```

`quote_literal` 函数将用户输入的 `O'Reilly` 转义为 `O''Reilly`，从而避免了 SQL 语法错误和潜在的 SQL 注入攻击。

**与 `quote_ident` 的区别：**

`quote_ident` 是另一个 PostgreSQL 函数，用于转义标识符（如表名、列名）。  它使用双引号 (`"`) 将标识符括起来，并转义标识符中的任何双引号。 `quote_literal` 用于转义字符串字面量，而 `quote_ident` 用于转义标识符。

**总结:**

`quote_literal` 是一个重要的安全工具，用于在动态构建 SQL 查询时转义字符串字面量，以防止 SQL 注入攻击。 在处理来自不可信来源的字符串值时，始终应该使用 `quote_literal` 或更安全的参数化查询。