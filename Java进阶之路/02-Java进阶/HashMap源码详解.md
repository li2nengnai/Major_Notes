---
created: 2026-06-08
source: https://javabetter.cn/collection/hashmap.html
tags: [java, 集合, hashmap, 源码]
优先级: ⭐⭐⭐
---

# HashMap 源码详解

> 来源：[二哥的Java进阶之路](https://javabetter.cn/collection/hashmap.html)
> 面试必问！理解 HashMap = 理解 Java 核心数据结构 + 哈希算法 + 红黑树

---

## 一、数据结构

### JDK 7 vs JDK 8 对比

| 项目 | JDK 7 | JDK 8 |
|------|-------|-------|
| 数据结构 | 数组 + 链表 | **数组 + 链表 + 红黑树** |
| 插入方式 | 头插法 | 尾插法 |
| 扩容后重新索引 | 重新计算 hash | 原位置 or 原位置+旧容量 |
| 链表转树阈值 | 无 | ≥8 |
| 树转链表阈值 | 无 | ≤6 |

### JDK 8 数据结构图

```
table[]（Node<K,V>[]，默认16）
├── [0] → null
├── [1] → Node → Node → Node（链表，长度>8转红黑树）
├── [2] → TreeNode（红黑树）
├── ...
└── [15] → Node
```

---

## 二、核心参数

```java
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;   // 16 默认容量
static final int MAXIMUM_CAPACITY = 1 << 30;           // 最大容量
static final float DEFAULT_LOAD_FACTOR = 0.75f;        // 默认负载因子
static final int TREEIFY_THRESHOLD = 8;                // 链表转树阈值
static final int UNTREEIFY_THRESHOLD = 6;              // 树转链表阈值
static final int MIN_TREEIFY_CAPACITY = 64;            // 最小树化容量

transient Node<K,V>[] table;    // 存储数组
transient int size;              // 键值对数量
int threshold;                   // 扩容阈值 = capacity * loadFactor
final float loadFactor;          // 负载因子
```

---

## 三、hash 原理

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

**为什么右移 16 位异或？**
- hashCode() 返回 32 位 int
- 高 16 位与低 16 位异或，混合高位低位信息
- 让散列更均匀，减少碰撞

**计算数组下标**：`(n - 1) & hash`（位运算取模，等价于 `hash % n`）

---

## 四、put 流程

```
put(key, value)
   ↓
计算 hash(key)
   ↓
table 为空？ → 是 → resize() 初始化
   ↓
计算下标 (n-1) & hash
   ↓
该位置为空？ → 是 → 直接 new Node 插入
   ↓
hash 冲突
   ├── key 相同？ → 覆盖 value
   ├── 红黑树节点？ → 红黑树插入
   └── 链表节点？ → 遍历链表
       ├── 找到相同 key → 覆盖
       └── 没找到 → 尾插新节点
           └── 链表长度 ≥ 8？ → treeifyBin() 转红黑树
   ↓
size++，超过 threshold？ → resize() 扩容
```

---

## 五、get 流程

```
get(key)
   ↓
计算 hash(key)
   ↓
table 不为空且对应位置有值？
   ├── 第一个节点匹配 → 直接返回
   ├── 红黑树 → getTreeNode()
   └── 链表 → 遍历查找
   ↓
都没找到 → 返回 null
```

---

## 六、扩容机制 resize()

**触发条件**：`size > threshold`（threshold = capacity × loadFactor）

**扩容步骤**：
1. 新容量 = 旧容量 × 2
2. 新阈值 = 新容量 × loadFactor
3. 创建新数组 `new Node[新容量]`
4. 重新分配旧元素到新数组

**JDK 8 优化**：重新 hash 时，元素要么在原位置，要么在原位置 + 旧容量
```java
// 关键代码
if ((e.hash & oldCap) == 0) {
    // 保持原索引位置
} else {
    // 移动到 原索引 + oldCap 位置
}
```

**为什么是 2 的幂？**
- `(n-1) & hash` 等价于 `hash % n`（位运算更快）
- 扩容后元素的位置计算更简单

---

## 七、线程安全性

**HashMap 不是线程安全的！**

### 并发问题
| 问题 | 说明 |
|------|------|
| JDK 7 扩容死循环 | 头插法 + 多线程扩容形成环形链表 |
| JDK 8 数据覆盖 | put 时两个线程同时拿到相同位置 |
| 数据不一致 | 一个线程 put 后另一个线程 get 不到 |

### 解决方案
```java
// 方案1：ConcurrentHashMap（推荐）
Map<String, String> map = new ConcurrentHashMap<>();

// 方案2：Collections.synchronizedMap
Map<String, String> map = Collections.synchronizedMap(new HashMap<>());

// 方案3：Hashtable（不推荐，性能差）
```

---

## 八、为什么容量是 2 的幂？

1. **位运算代替取模**：`(n-1) & hash` 性能优于 `hash % n`
2. **扩容后元素重定位简单**：只需检查新增 bit 是 0 还是 1
3. **散列更均匀**：`n-1` 的二进制全是 1，与 hash 与运算后分布更均匀

---

## 💡 面试高频题

1. **HashMap 底层数据结构？** → JDK8:数组+链表+红黑树
2. **为什么负载因子是 0.75？** → 空间与时间的平衡，泊松分布下链表长度概率极低
3. **链表转红黑树的阈值为什么是 8？** → 泊松分布下链表长度 8 的概率极小（0.00000006）
4. **HashMap 允许 null key 吗？** → 允许，null key 的 hash 为 0，放在 table[0]
5. **HashMap 和 Hashtable 区别？** → 线程安全、null 允许、性能、扩容机制
6. **HashMap 如何解决 hash 冲突？** → 链地址法（链表+红黑树）
7. **ConcurrentHashMap 怎么保证线程安全？** → CAS + synchronized + volatile
