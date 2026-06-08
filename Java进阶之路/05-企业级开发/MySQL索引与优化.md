---
created: 2026-06-08
source: https://javabetter.cn/mysql/
tags: [java, mysql, 数据库, 索引]
优先级: ⭐⭐⭐
---

# MySQL 索引与优化

> 来源：[二哥的Java进阶之路](https://javabetter.cn/mysql/)

---

## 一、索引原理

### B+ 树结构
```
                 [根节点：键值指针]
                /        \
         [内部节点]      [内部节点]
          /     \         /     \
      [叶子节点] [叶子节点] [叶子节点] [叶子节点]
       data1    data2     data3    data4
```

**B+ 树特点**：
- 非叶子节点只存键值，不存数据 → 更矮更宽，IO 少
- 叶子节点包含所有数据，且形成有序链表 → 范围查询高效
- 扇出高（通常 1000+），3 层可存千万级数据

### 聚簇索引 vs 非聚簇索引

| 对比 | 聚簇索引 | 非聚簇索引（二级索引） |
|------|---------|-------------------|
| 数据存储 | 索引即数据 | 叶子存主键值 |
| 数量 | 1 个（主键） | 多个 |
| 回表 | 不需要 | 需要（查主键后再查聚簇索引） |
| 覆盖索引 | — | 索引包含所需全部字段，不回表 |

**MyISAM vs InnoDB**：
- MyISAM：非聚簇，索引文件与数据文件分离
- InnoDB（默认）：聚簇，表即索引

---

## 二、索引类型

```sql
-- 主键索引
PRIMARY KEY (id)

-- 普通索引
INDEX idx_name (name)

-- 唯一索引（允许 NULL，但值唯一）
UNIQUE INDEX idx_email (email)

-- 联合索引（最左前缀原则）
INDEX idx_name_age (name, age)

-- 全文索引（MyISAM/InnoDB 5.6+）
FULLTEXT INDEX idx_content (content)
```

### 联合索引最左前缀原则
```sql
INDEX idx_a_b_c (a, b, c)

-- ✅ 能用索引
WHERE a = 1
WHERE a = 1 AND b = 2
WHERE a = 1 AND b = 2 AND c = 3
WHERE a = 1 AND c = 3  -- MySQL 优化后尽量用 a

-- ❌ 不能使用索引
WHERE b = 2          -- 跳过了 a
WHERE b = 2 AND c = 3 -- 跳过了 a
```

---

## 三、EXPLAIN 执行计划

```sql
EXPLAIN SELECT * FROM user WHERE name = '张三';
```

**关键字段**：

| 字段 | 说明 | 好→差 |
|------|------|--------|
| type | 访问类型 | const > ref > range > index > ALL |
| key | 实际使用索引 | — |
| rows | 扫描行数 | 越小越好 |
| Extra | 额外信息 | Using index（好）> Using filesort（差） |
| possible_keys | 可能使用的索引 | — |

**type 详解**：
- `const`：主键/唯一索引等值查询
- `ref`：普通索引等值查询
- `range`：范围查询（> < between in）
- `index`：扫描整个索引树
- `ALL`：全表扫描（⚠️ 危险）

---

## 四、SQL 优化策略

### 4.1 索引优化
- 选择区分度高的列建索引
- 使用覆盖索引避免回表
- 避免在索引列上使用函数、运算
- 联合索引将区分度高的放前面

### 4.2 查询优化
```sql
-- ❌ 避免 SELECT *（可能用不上覆盖索引）
SELECT * FROM user WHERE name = '张三';

-- ✅ 只查需要的字段
SELECT id, name FROM user WHERE name = '张三';

-- ❌ 避免隐式类型转换
WHERE phone = 123456  -- phone 是 varchar，不走索引

-- ✅
WHERE phone = '123456'

-- ❌ 避免左模糊（不走索引）
WHERE name LIKE '%张三'

-- ✅ 右模糊可以走索引
WHERE name LIKE '张三%'
```

### 4.3 分页优化
```sql
-- ❌ 深分页效率低
SELECT * FROM user LIMIT 100000, 10;

-- ✅ 子查询优化（延迟关联）
SELECT * FROM user 
WHERE id > (SELECT id FROM user ORDER BY id LIMIT 100000, 1) 
LIMIT 10;
```

### 4.4 常见反例
- 不要在索引列上使用 `!=`、`NOT IN`、`IS NOT NULL`
- 避免大表 JOIN，最多 3 张表
- 大表 COUNT 用二级索引（比主键小）
- 批量插入用 `VALUES (...), (...), (...)`

---

## 五、事务与隔离级别

### ACID
| 特性 | 说明 |
|------|------|
| 原子性（A） | 全部成功或全部回滚 |
| 一致性（C） | 事务前后数据一致 |
| 隔离性（I） | 事务之间互不干扰 |
| 持久性（D） | 提交后永久保存 |

### 隔离级别

| 级别 | 脏读 | 不可重复读 | 幻读 |
|------|:----:|:---------:|:----:|
| READ UNCOMMITTED | ✅ | ✅ | ✅ |
| READ COMMITTED（Oracle 默认） | ❌ | ✅ | ✅ |
| REPEATABLE READ（MySQL 默认） | ❌ | ❌ | ✅ |
| SERIALIZABLE | ❌ | ❌ | ❌ |

**MySQL 可重复读如何解决幻读？** → MVCC（快照读）+ Gap Lock / Next-Key Lock（当前读）

---

## 六、MVCC（多版本并发控制）

**核心**：每行记录隐藏字段（DB_TRX_ID 事务ID、DB_ROLL_PTR 回滚指针） + undo log + ReadView

**ReadView 可见性判断**：
```
creator_trx_id（创建者的事务ID）
up_limit_id（未提交事务最小ID）
low_limit_id（下一个事务ID）
```

---

## 💡 面试高频题

1. **B+ 树和 B 树的区别？** → B+树非叶子不存数据，叶子有链表，更适合范围查询
2. **为什么 MySQL 用 B+ 树而不是 B 树？** → 扇出更高、范围查询更快、IO 更少
3. **最左前缀原则是什么？** → 联合索引从最左列开始匹配，跳过某列后失效
4. **为什么要用覆盖索引？** → 避免回表，减少 IO
5. **事务隔离级别怎么实现的？** → 读未提交直接读、读已提交/可重复读用 MVCC
6. **慢查询怎么排查？** → 开启慢查询日志 → EXPLAIN 分析 → 优化 SQL/索引
7. **分库分表 vs 分区表？** → 分库分表解决单机瓶颈，分区表是表内逻辑拆分
