---
title: String 深入理解
created: 2026-06-08
source: https://javabetter.cn/string/immutable.html
tags: [技术/Java/基础, 技术/Java/数据结构, 笔记/进阶之路]
---

# [[String]] 深入理解

## 核心概念

### String 不可变性

```java
public final class String {
    private final char value[];  // final 修饰，不可变
    private int hash;             // 缓存 hashCode
}
```

**为什么不可变？** `final` 类不可继承，`char[]` 用 `final` 修饰且不暴露内部。

**不可变的好处**：常量池复用、线程安全、hashCode 可缓存（适合做 [[HashMap]] 的 key）。

### 字符串常量池

```java
String s1 = "hello";              // 常量池
String s2 = "hello";              // 复用常量池
String s3 = new String("hello");  // 堆中新对象
s1 == s2  → true   （常量池同一对象）
s1 == s3  → false  （堆中不同对象）
s1.equals(s3) → true（内容相同）
```

### 字符串拼接

```java
// 常量拼接：编译期优化
String s = "a" + "b" + "c";  // 编译为 "abc"

// 变量拼接：底层 StringBuilder
String s = a + b + c;

// ⚠️ 循环拼接不要用 +
String result = "";
for (String s : list) { result += s; }  // ❌ 每次 new StringBuilder

// ✅ 用 StringBuilder
StringBuilder sb = new StringBuilder();
for (String s : list) { sb.append(s); }
```

### StringBuilder vs StringBuffer

| 比较 | StringBuilder | StringBuffer |
|------|--------------|-------------|
| 线程安全 | ❌ | ✅（synchronized） |
| 性能 | 高 | 中 |
| 推荐 | 单线程首选 | 多线程共享 |

## 踩坑记录

> [!warning] new String("abc") 创建了几个对象
> - 如果常量池没有 "abc"：创建 2 个（常量池 + 堆）
> - 如果常量池已有 "abc"：创建 1 个（堆）

> [!warning] 频繁拼接字符串性能差
> ```java
> // ❌ 每次 += 都 new StringBuilder
> // ✅ 显式用 StringBuilder
> ```

## 知识依赖图

先掌握 [[../../Java SE 考点复习/01-变量与数据类型|数据类型]] → 再学本篇 → 延伸 [[../../Java SE 考点复习/09-String与ArrayList|String 考点]] · [[../../Java SE 考点复习/12-常用API|StringBuilder]]

---

#技术/Java/基础 #技术/Java/数据结构 #笔记/进阶之路
