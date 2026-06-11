---
title: 常用 API
created: 2026-06-08
tags: [技术/Java/API, 技术/Java/进阶, 笔记/考点复习]
---

# [[Java API|常用 API]]

## 📌 课程模块

pk011-pk012 · 包装类/StringBuilder/时间/BigDecimal/Arrays · **理解考点**

## 核心概念

### [[包装类]]

基本类型 → 引用类型（如 `int` → `Integer`），用于泛型和类型转换。

```java
Integer a = 12;               // 自动装箱
int b = a;                    // 自动拆箱
ArrayList<Integer> list = new ArrayList<>();  // 泛型必须用包装类

// 字符串 ↔ 基本类型
int age = Integer.parseInt("29");      // String → int
String s = Integer.toString(23);       // int → String
String s2 = 23 + "";                   // 最简便的方式
```

### [[StringBuilder]]

可变字符串，用于频繁拼接的场景，**链式编程**：

```java
StringBuilder sb = new StringBuilder("Hello");
sb.append(" ").append("World").append("!");  // 链式
sb.reverse();                                 // 反转
String result = sb.toString();               // 转 String
```

### [[BigDecimal]]

解决浮点数精度丢失问题：

```java
System.out.println(0.1 + 0.2);  // 0.30000000000000004 ❌

BigDecimal a = BigDecimal.valueOf(0.1);
BigDecimal b = BigDecimal.valueOf(0.2);
BigDecimal c = a.add(b);        // 精确的 0.3
```

### [[Date]] 和 [[SimpleDateFormat]]

```java
Date d = new Date();                     // 当前时间
long time = d.getTime();                 // 毫秒值
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String rs = sdf.format(d);               // Date → 字符串
Date d2 = sdf.parse("2028-11-11 10:24"); // 字符串 → Date
```

### [[Arrays]] 工具类

```java
Arrays.toString(arr);              // 数组转字符串
Arrays.copyOfRange(arr, 1, 4);    // 拷贝部分
Arrays.copyOf(arr, 10);           // 拷贝并指定长度
Arrays.sort(arr);                  // 排序
```

## 踩坑记录

> [!warning] 包装类 `==` 比较的坑
> ```java
> Integer a = 127;  // 自动装箱，用到缓存
> Integer b = 127;
> System.out.println(a == b);  // true（-128~127 范围内有缓存）
>
> Integer c = 128;
> Integer d = 128;
> System.out.println(c == d);  // false（超出缓存范围）
> ```
> **规则**：包装类比较用 `equals()`，不要用 `==`。

> [!warning] BigDecimal 的除法要指定精度
> ```java
> BigDecimal a = BigDecimal.valueOf(0.1);
> BigDecimal b = BigDecimal.valueOf(0.3);
> a.divide(b);  // ❌ ArithmeticException: 除不尽！
> a.divide(b, 2, RoundingMode.HALF_UP);  // ✅ 0.33
> ```

> [!warning] BigDecimal 创建方式
> ```java
> BigDecimal d1 = new BigDecimal(0.1);   // ❌ 不精确！
> BigDecimal d2 = BigDecimal.valueOf(0.1); // ✅ 推荐
> ```

## 拓展延伸

→ [[09-String与ArrayList]] · [[../../Java进阶之路/01-Java基础/String深入理解]]

## 知识依赖图

先掌握 [[09-String与ArrayList|String]] · [[01-变量与数据类型|基本类型]] → 再学本篇 → 延伸 [[14-集合框架|集合框架]]

---

#技术/Java/API #技术/Java/进阶 #笔记/考点复习
