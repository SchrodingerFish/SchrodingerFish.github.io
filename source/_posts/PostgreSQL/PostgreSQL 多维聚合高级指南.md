---
title: PostgreSQL 多维聚合高级指南
date: 2026-07-29 14:54:00
categories:
  - PostgreSQL
tags:
  - PostgreSQL
---

# PostgreSQL 多维聚合高级指南：GROUPING SETS、ROLLUP、CUBE 与 GROUPING()

在 PostgreSQL（及标准 SQL）中，处理多维度汇总报表时，传统的 `GROUP BY` 配合 `UNION ALL` 往往会导致代码冗长且性能较差。PostgreSQL 提供了 **`GROUPING SETS`** 及其衍生语法 **`ROLLUP`** 与 **`CUBE`**，能够显著简化查询逻辑并优化数据库执行效率。

本文档详细整理了这些高级聚合特性的使用场景、核心优势、语法示例及空值处理技巧。

---

## 一、核心概念与应用场景

### 1.1 为什么需要高级聚合？
在商业智能（BI）、数据仓库及财务报表分析中，经常需要**在同一个查询中同时生成多个不同颗粒度（维度组合）的汇总结果**。

例如，对于一张销售记录表，业务方可能希望在一份报表中同时看到：
- **细粒度数据**：按“区域 + 年份”汇总
- **小计数据**：仅按“区域”汇总、仅按“年份”汇总
- **总计数据**：全公司总计（不分组）

### 1.2 核心优势
1. **性能提升（单次扫描）**：使用 `UNION ALL` 会导致数据库多次扫描基础表；而 `GROUPING SETS` 允许 PostgreSQL **仅扫描一次数据**，并在内存中并发计算多个维度的聚合。
2. **代码简洁**：避免编写大量重复的过滤条件、关联逻辑与冗长的 `UNION ALL`。

---

## 二、基础表结构与测试数据

为方便后续示例说明，假设有如下结构的销售记录表 `sales`：

```sql
-- 创建测试表
CREATE TABLE sales (
    id SERIAL PRIMARY KEY,
    region VARCHAR(50),
    year INT,
    month INT,
    amount NUMERIC(10, 2)
);

-- 插入测试数据
INSERT INTO sales (region, year, month, amount) VALUES
('North', 2023, 1, 100.00),
('North', 2023, 2, 150.00),
('North', 2024, 1, 200.00),
('South', 2023, 1, 300.00),
('South', 2024, 1, 400.00),
(NULL,    2024, 2, 50.00); -- 包含一条 region 为 NULL 的真实数据
```

---

## 三、GROUP BY GROUPING SETS（自定义维度集合）

### 3.1 语法说明
`GROUPING SETS` 允许你**显式指定**需要计算聚合的所有维度组合。每个括号内部代表一个维度集合，空括号 `()` 代表不分组（全表总计）。

### 3.2 传统 UNION ALL vs GROUPING SETS

#### 传统做法（使用 UNION ALL，扫描 4 次）
```sql
SELECT region, year, SUM(amount) AS total_sales FROM sales GROUP BY region, year
UNION ALL
SELECT region, NULL AS year, SUM(amount) AS total_sales FROM sales GROUP BY region
UNION ALL
SELECT NULL AS region, year, SUM(amount) AS total_sales FROM sales GROUP BY year
UNION ALL
SELECT NULL AS region, NULL AS year, SUM(amount) AS total_sales FROM sales;
```

#### 推荐做法（使用 GROUPING SETS，仅扫描 1 次）
```sql
SELECT 
    region, 
    year, 
    SUM(amount) AS total_sales
FROM sales
GROUP BY GROUPING SETS (
    (region, year),  -- 维度 1：区域 + 年份
    (region),        -- 维度 2：仅按区域
    (year),          -- 维度 3：仅按年份
    ()               -- 维度 4：全局总计
)
ORDER BY region, year;
```

---

## 四、ROLLUP 与 CUBE 快捷语法

为了简化常见的维度组合模式，PostgreSQL 提供了 `ROLLUP` 和 `CUBE` 两种语法糖。

### 4.1 ROLLUP：层级递进汇总

#### 适用场景
适用于具有**上下级层级关系**的维度，如：`国家 -> 省份 -> 城市`，或 `年 -> 月 -> 日`。

#### 扩展规律
`ROLLUP (A, B, C)` 会按照**从右向左**逐层剥离降维，等价于：
```sql
GROUPING SETS (
    (A, B, C),
    (A, B),
    (A),
    ()
)
```

#### 示例：按 年 -> 月 卷起汇总
```sql
SELECT 
    year, 
    month, 
    SUM(amount) AS total_sales
FROM sales
GROUP BY ROLLUP (year, month)
ORDER BY year, month;
```

**执行结果：**
| year | month | total_sales | 说明 |
| :--- | :--- | :--- | :--- |
| 2023 | 1 | 400.00 | 2023年1月明细 |
| 2023 | 2 | 150.00 | 2023年2月明细 |
| 2023 | *NULL* | **550.00** | **2023年年度小计** |
| 2024 | 1 | 600.00 | 2024年1月明细 |
| 2024 | 2 | 50.00 | 2024年2月明细 |
| 2024 | *NULL* | **650.00** | **2024年年度小计** |
| *NULL* | *NULL* | **1200.00** | **全公司总计** |

---

### 4.2 CUBE：多维全排列交叉汇总

#### 适用场景
适用于需要对所有维度进行**交叉组合分析**（即魔方分析）的场景。

#### 扩展规律
对于 $N$ 个字段，`CUBE (A, B)` 会生成 $2^N$ 种所有可能的组合，等价于：
```sql
GROUPING SETS (
    (A, B),
    (A),
    (B),
    ()
)
```

#### 示例：按 区域 与 年份 全交叉汇总
```sql
SELECT 
    region, 
    year, 
    SUM(amount) AS total_sales
FROM sales
GROUP BY CUBE (region, year)
ORDER BY region, year;
```

---

## 五、空值识别与标签化：GROUPING() 函数

### 5.1 痛点问题
在多维聚合的结果中，聚合留空的列会填充 `NULL`。但如果原始表中该字段本身就包含 `NULL`（如未分类区域），就无法区分该 `NULL` 是**原始数据**还是**聚合生成的小计**。

### 5.2 GROUPING() 函数原理
- `GROUPING(column_name)` 返回 `1`：表示该列**已被聚合剥离**（即当前行是该列的汇总行/小计行）。
- `GROUPING(column_name)` 返回 `0`：表示该列**属于正常分组数据**（即使值本身是 NULL）。

### 5.3 进阶实践：美化报表输出

结合 `GROUPING()` 与 `CASE WHEN`，可以将聚合产生的 `NULL` 转换为清晰的报表标签：

```sql
SELECT 
    -- 区分汇总 NULL 与原始数据 NULL
    CASE 
        WHEN GROUPING(region) = 1 THEN '【所有区域总计】' 
        ELSE COALESCE(region, '未知区域') 
    END AS region_label,
    
    CASE 
        WHEN GROUPING(year) = 1 THEN '【所有年份总计】' 
        ELSE CAST(year AS VARCHAR) 
    END AS year_label,
    
    -- 提取 GROUPING 标志位（可选，用于前端逻辑判断）
    GROUPING(region) AS is_region_aggregated,
    GROUPING(year) AS is_year_aggregated,
    
    SUM(amount) AS total_sales
FROM sales
GROUP BY CUBE (region, year)
ORDER BY GROUPING(region), region, GROUPING(year), year;
```

**格式化后的输出效果：**
| region_label | year_label | is_region_aggregated | is_year_aggregated | total_sales |
| :--- | :--- | :--- | :--- | :--- |
| North | 2023 | 0 | 0 | 250.00 |
| North | 2024 | 0 | 0 | 200.00 |
| North | **【所有年份总计】** | 0 | **1** | **450.00** |
| South | 2023 | 0 | 0 | 300.00 |
| South | 2024 | 0 | 0 | 400.00 |
| South | **【所有年份总计】** | 0 | **1** | **700.00** |
| 未知区域 | 2024 | 0 | 0 | 50.00 |
| 未知区域 | **【所有年份总计】** | 0 | **1** | **50.00** |
| **【所有区域总计】** | 2023 | **1** | 0 | **550.00** |
| **【所有区域总计】** | 2024 | **1** | 0 | **650.00** |
| **【所有区域总计】** | **【所有年份总计】** | **1** | **1** | **1200.00** |

---

## 六、总结与选型指南

| 语法特性 | 组合公式/数量 | 适用场景 | 最佳实践建议 |
| :--- | :--- | :--- | :--- |
| **GROUPING SETS** | 完全自定义组合 | 复杂的非标准多维报表 | 仅指定业务所需的特定维度组合，避免无效计算 |
| **ROLLUP** | $N + 1$ 种组合 | 具有层级包含关系的维度（年/月/日、国/省/市） | 字段顺序至关重要，从左至右代表从高层级到低层级 |
| **CUBE** | $2^N$ 种组合 | 维度较少且需要全方位交叉分析 | 谨慎使用于多字段（如 $N > 5$），防止组合爆炸影响性能 |
| **GROUPING()** | N/A（辅助函数） | 报表展现美化与空值判定 | 始终配合 `CASE WHEN` 将聚合 `NULL` 转化为可读提示 |
