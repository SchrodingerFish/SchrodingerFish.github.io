---
title: NULLIF 函数 在PostgreSQL中的应用
date: 2026-04-11 17:43:00
categories:
  - PostgreSQL
tags:
  - PostgreSQL
---
## 🤖 Assistant

`NULLIF` 是 PostgreSQL 中一个非常实用的**空值处理函数**，用于将两个相等的值转换为 `NULL`，常用于避免除零错误、消除重复值、标准化数据等场景。

---

### ✅ 语法

```sql
NULLIF(expression1, expression2)
```

- **返回值逻辑**：
- 如果 `expression1 = expression2`，返回 `NULL`
- 否则，返回 `expression1`

> ⚠️ 注意：比较时遵循 `=` 的规则（即 `NULL = NULL` 返回 `NULL`，不是 `TRUE`，因此 `NULLIF(NULL, NULL)` 仍为 `NULL`，但不是因为相等，而是因为不满足 `=` 返回 `TRUE` 的条件）。

---

### 📌 常见使用场景

#### 1️⃣ 避免除零错误（最常用！）

```sql
SELECT amount / NULLIF(count, 0) AS avg_amount
FROM sales;
```
- 当 `count = 0` 时，`NULLIF(count, 0)` 返回 `NULL`
- 任意数除以 `NULL` 结果是 `NULL`（而不是报错 `division by zero`）

> ✅ 比用 `CASE WHEN count = 0 THEN NULL ELSE amount / count END` 更简洁。

---

#### 2️⃣ 将特定值转换为 `NULL`（如空字符串、0、-1 等）

```sql
-- 假设数据库中用 0 表示“未指定”，想转为 NULL
SELECT NULLIF(status_code, 0) AS status
FROM orders;

-- 将空字符串转为 NULL
SELECT NULLIF(TRIM(name), '') AS valid_name
FROM users;
```

---

#### 3️⃣ 处理聚合中的默认值（如 `COUNT(*)` 防止 0 代替 NULL）

```sql
-- 希望：没有订单的用户显示 NULL 而不是 0
SELECT 
  u.id,
  COUNT(o.id) AS order_count,
  NULLIF(COUNT(o.id), 0) AS non_zero_orders  -- 非必选，但有时用于区分 0 和实际计数
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id;
```

> 更常见的是结合 `COALESCE(NULLIF(...), '')` 使用。

---

#### 4️⃣ 搭配 `COALESCE` 实现条件默认值

```sql
-- 如果 name 是 NULL 或空字符串，用 'Anonymous'
SELECT COALESCE(NULLIF(TRIM(name), ''), 'Anonymous') AS display_name
FROM users;
```
- 先用 `NULLIF(TRIM(name), '')` 把空字符串转成 `NULL`
- 再用 `COALESCE(..., 'Anonymous')` 把 `NULL` 换成默认值

---

### 🔍 注意事项

| 情况 | `NULLIF(x, y)` 返回 |
|------|--------------------|
| `x = y`（非 NULL，且 `=` 比较为 `TRUE`） | `NULL` |
| `x != y`（或比较为 `FALSE`） | `x` |
| `x IS NULL` 或 `y IS NULL` | `x`（因为 `NULL = NULL` → `UNKNOWN` ≠ `TRUE`）|
| `NULLIF(NULL, NULL)` | **返回 `NULL`**（因为 `NULL = NULL` 不成立，所以返回第一个参数 `NULL`）|

---

### 🧪 示例验证

```sql
SELECT 
  NULLIF(10, 10) AS same,         -- → NULL
  NULLIF(10, 5)  AS diff,         -- → 10
  NULLIF(NULL, NULL) AS null1,    -- → NULL
  NULLIF(NULL, 1)   AS null_val,  -- → NULL
  NULLIF(1, NULL)   AS val_null;  -- → 1
```

---

### ✅ 小结

| 函数 | 作用 | 推荐用途 |
|------|------|----------|
| `NULLIF(a, b)` | 当 `a = b` 时返 `NULL`，否则返 `a` | 避免除零、标准化空值、配合 `COALESCE` 处理默认值 |

用好 `NULLIF` 能让 SQL 更健壮、简洁、可读性高 💡