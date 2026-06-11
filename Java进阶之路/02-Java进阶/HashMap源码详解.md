---
title: HashMap 源码详解
created: 2026-06-08
source: https://javabetter.cn/collection/hashmap.html
tags: [技术/Java/集合, 技术/Java/源码, 笔记/进阶之路]
---

# [[HashMap]] 源码详解

## 核心概念

### 数据结构（JDK8）

**数组 + 链表 + 红黑树**。数组默认 16，链表长度 ≥8 转红黑树，红黑树 ≤6 转回链表。

### 核心参数

```java
static final int DEFAULT_INITIAL_CAPACITY = 16;  // 默认容量
static final float DEFAULT_LOAD_FACTOR = 0.75f;   // 负载因子
static final int TREEIFY_THRESHOLD = 8;           // 树化阈值
```

### hash 算法

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    // 高16位与低16位异或，让散列更均匀
}
```

数组下标计算：`(n - 1) & hash`（位运算取模，等价于 `hash % n`）

### put 流程

```
计算 hash → 数组为空？→ resize() 初始化
         → 下标为空？→ 直接插入
         → hash 冲突
             ├── key 相同 → 覆盖 value
             ├── 红黑树   → 树插入
             └── 链表     → 尾插，长度≥8转红黑树
         → size++ → 超过 threshold？→ resize()
```

### 扩容

容量 × 2。JDK8 优化：元素要么在原位，要么在原位 + 旧容量。

### 线程不安全

JDK7 头插法多线程可能死循环，JDK8 尾插法可能数据覆盖。并发用 `ConcurrentHashMap`。

## 踩坑记录

> [!warning] HashMap 线程不安全
> 多线程场景用 `ConcurrentHashMap`，不要用 `Hashtable`（性能差）。

> [!warning] 自定义对象做 key 要重写 hashCode() 和 equals()
> 不重写的话，内容相同的两个对象会被当成不同的 key。

## 知识依赖图

先掌握 [[集合框架总览|集合框架]] → 再学本篇 → 延伸 [[../../Java SE 考点复习/14-集合框架|集合框架考点]]

---

#技术/Java/集合 #技术/Java/源码 #笔记/进阶之路
