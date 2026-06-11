---
title: Scanner 键盘录入
created: 2026-06-08
tags: [技术/Java/基础, 技术/Java/IO, 笔记/考点复习]
---

# [[Scanner]] 键盘录入

## 📌 课程模块

pk002 · 键盘输入交互 · **理解考点**

## 核心概念

[[Scanner]] 是 Java 提供的键盘输入工具，三步法：

1. **导包**：`import java.util.Scanner;`
2. **创建对象**：`Scanner sc = new Scanner(System.in);`
3. **调用方法**：`sc.nextInt()` / `sc.next()`

程序执行到 `nextInt()` 时会**阻塞等待**，直到用户按回车。

## 语法格式

```java
Scanner sc = new Scanner(System.in);
int age = sc.nextInt();      // 读取整数
String name = sc.next();     // 读取字符串（遇到空格停止）
String line = sc.nextLine(); // 读取整行（可包含空格）
```

## 踩坑记录

> [!warning] nextInt() 后 nextLine() 读不到内容
> **现象**：输入年龄后按回车，名字没等输入就直接跳过了
> **原因**：`nextInt()` 只读取数字，把换行符留在了缓冲区。后面的 `nextLine()` 直接读到了这个空行。
> ```java
> Scanner sc = new Scanner(System.in);
> int age = sc.nextInt();      // 输入 18 按回车，18 被读取，\n 留在缓冲区
> String name = sc.nextLine(); // 直接读到 \n，返回空字符串！
> ```
> **解决**：
> ```java
> int age = sc.nextInt();
> sc.nextLine();               // 吃掉残留的换行符
> String name = sc.nextLine(); // 正常读取
> ```
> 或者统一用 `nextLine()` 读取后手动转换：
> ```java
> int age = Integer.parseInt(sc.nextLine());
> String name = sc.nextLine();
> ```

## 实战案例

```java
Scanner sc = new Scanner(System.in);
System.out.println("请输入年龄：");
int age = sc.nextInt();
System.out.println("请输入姓名：");
String name = sc.next();
System.out.println(name + "的年龄是" + age);
```

## 拓展延伸

→ [[05-流程控制]]（配合 Scanner 做交互菜单）· [[../../Java进阶之路/01-Java基础/异常处理|异常处理]]（输入非数字时的异常处理）

## 知识依赖图

先掌握 [[01-变量与数据类型|变量]] → 再学本篇 → 延伸 [[05-流程控制|交互菜单]] · [[13-异常处理|异常捕获]]

---

#技术/Java/基础 #技术/Java/IO #笔记/考点复习
