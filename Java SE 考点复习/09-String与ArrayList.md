---
title: String 与 ArrayList
created: 2026-06-08
tags: [技术/Java/基础, 技术/Java/数据结构, 笔记/考点复习]
---

# [[String]] 与 [[ArrayList]]

## 📌 课程模块

pk008 · String 常用方法/字符串池/ArrayList 集合 · **高频考点**

## 核心概念

### [[String]] 两种创建方式的区别

```java
String s1 = "hello";             // 字符串常量池（复用）
String s2 = "hello";             // 复用常量池中的对象
String s3 = new String("hello"); // 堆中新对象

s1 == s2  → true  （常量池同一对象）
s1 == s3  → false （堆中不同对象）
s1.equals(s3) → true（内容相同）
```

**原则：内容比较永远用 `equals()`，别用 `==`。**

### [[String]] 不可变性

String 创建后内容不可变，每次修改都返回新对象：

```java
String s = "abc";
s.toUpperCase();
System.out.println(s);  // "abc"（没变！要重新赋值）
```

频繁拼接时用 [[StringBuilder]] 替代：

```java
// ❌ 每次 + 都 new StringBuilder
String s = "";
for (String x : list) { s += x; }

// ✅ 一个 StringBuilder 搞定
StringBuilder sb = new StringBuilder();
for (String x : list) { sb.append(x); }
String s = sb.toString();
```

### [[ArrayList]] 集合

长度可变的数组，只能存引用类型：

```java
ArrayList<String> list = new ArrayList<>();
list.add("Java");        // 末尾添加
list.get(0);             // 获取
list.size();             // 大小
list.remove(0);          // 按索引删除
list.remove("Java");     // 按元素删除（只有第一次出现的）
list.set(0, "新值");     // 修改
```

## 语法格式

### String 常用方法

```java
s.length()                  // 长度
s.charAt(i)                 // 第 i 个字符
s.equals("xx")              // 比较内容
s.equalsIgnoreCase("XX")   // 忽略大小写比较
s.substring(0, 8)           // 截取 [0,8)
s.replace("旧", "新")       // 替换
s.contains("xx")            // 是否包含
s.startsWith("x")           // 是否以 x 开头
s.split(",")                // 按逗号分割
```

### 遍历方式

```java
// 方式1：charAt
for (int i = 0; i < s.length(); i++) {
    char ch = s.charAt(i);
}

// 方式2：toCharArray
for (char ch : s.toCharArray()) { }
```

## 踩坑记录

> [!warning] `==` 比较 String 内容
> **现象**：`s1 == s2` 有时 true 有时 false，结果不确定
> ```java
> String s1 = "abc";
> String s2 = "abc";
> String s3 = new String("abc");
> System.out.println(s1 == s2);   // true（常量池）
> System.out.println(s1 == s3);   // false（堆中）
> ```
> **规则**：`==` 比较引用地址，`equals()` 比较内容。

> [!warning] ArrayList 泛型不支持基本类型
> ```java
> ArrayList<int> list = new ArrayList<>();  // ❌ 编译错误
> ArrayList<Integer> list = new ArrayList<>(); // ✅ 用包装类
> ```
> 基本类型泛型需用包装类：`Integer`、`Double`、`Boolean` 等。

## 实战案例

```java
// 字符串遍历统计字符
String s = "hello";
int count = 0;
for (int i = 0; i < s.length(); i++) {
    if (s.charAt(i) == 'l') count++;
}
System.out.println("l 出现了 " + count + " 次");
```

## 拓展延伸

→ [[14-集合框架]] · [[12-常用API|StringBuilder]] · [[../../Java进阶之路/01-Java基础/String深入理解]]

## 知识依赖图

先掌握 [[06-数组|数组]] · [[08-面向对象基础|OOP]] → 再学本篇 → 延伸 [[14-集合框架|集合框架]] · [[12-常用API|StringBuilder/StringBuffer]]

---

#技术/Java/基础 #技术/Java/数据结构 #笔记/考点复习
