---
title: MySQL 索引与优化
created: 2026-06-08
source: https://javabetter.cn/mysql/
tags: [技术/数据库/MySQL, 技术/数据库/优化, 笔记/进阶之路]
---

# [[MySQL]] 索引与优化

## 核心概念

### B+ 树

非叶子节点只存键值，叶子节点存数据 + 有序链表。3 层可存千万级数据，范围查询高效。

### 聚簇索引 vs 非聚簇索引

| 对比 | 聚簇索引 | 非聚簇索引（二级索引） |
|------|---------|-------------------|
| 数据存储 | 索引即数据 | 叶子存主键 |
| 数量 | 1 个（主键） | 多个 |
| 回表 | 不需要 | 需要 |

### 索引类型

```sql
PRIMARY KEY (id)              -- 主键索引
INDEX idx_name (name)         -- 普通索引
UNIQUE INDEX idx_email (email) -- 唯一索引
INDEX idx_a_b_c (a, b, c)     -- 联合索引（最左前缀原则）
```

### 最左前缀原则

```sql
INDEX idx_a_b_c (a, b, c)
WHERE a = 1 AND b = 2    → ✅ 用索引
WHERE b = 2              → ❌ 跳过了 a，不走索引
```

### EXPLAIN 关键字段

`type`：`const > ref > range > index > ALL`（ALL = 全表扫描，危险）

## 踩坑记录

> [!warning] 索引失效场景
> - `WHERE name LIKE '%xx'`（左模糊）
> - `WHERE YEAR(date) = 2026`（函数操作）
> - `WHERE phone = 123456`（隐式类型转换）
> - `WHERE name != 'xx'`（不等于）

> [!warning] SELECT * 可能让覆盖索引失效
> `SELECT *` 需要回表查所有字段，`SELECT id, name` 只需要扫索引。

## 知识依赖图

先掌握 SQL 基础 → 再学本篇 → 延伸 [[../05-企业级开发/mybatis/MyBatis从零入门|MyBatis]]

---

#技术/数据库/MySQL #技术/数据库/优化 #笔记/进阶之路
