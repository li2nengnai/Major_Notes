---
title: static 关键字
created: 2026-06-08
tags: [技术/Java/OOP, 技术/Java/基础, 笔记/考点复习]
---

# [[static]] 关键字

## 📌 课程模块

pk009 · 静态变量/静态方法/工具类/静态代码块/单例模式 · **高频考点**

## 核心概念

### 类变量 vs 实例变量

| 类型 | 修饰 | 归属 | 访问方式 | 内存时机 |
|------|------|------|---------|---------|
| 类变量 | `static` | 类（所有对象共享） | `类名.变量名` | 类加载时 |
| 实例变量 | 无 | 对象（各自独立） | `对象.变量名` | `new` 时 |

```java
Student.schoolName = "北大";  // 类变量，类名访问

Student s1 = new Student();
Student s2 = new Student();
s1.name = "张三";
s2.name = "李四";
System.out.println(s1.name);  // 张三（实例变量独立）
```

**类变量共享一份**，一个对象改了其他对象也能看到。

### 工具类

所有方法都是静态的实用类，构造器私有，不允许创建对象（如 [[Arrays]]、[[Collections]]）。

```java
public class MyUtil {
    private MyUtil() { }  // 不让 new
    public static void hello() { }
}
MyUtil.hello();  // 直接类名调用
```

### [[单例模式]]（饿汉式）

确保一个类只有一个对象：

```java
public class A {
    private static A instance = new A();  // 类加载时创建
    private A() { }                       // 构造器私有
    public static A getInstance() { return instance; }
}

A a1 = A.getInstance();
A a2 = A.getInstance();
System.out.println(a1 == a2);  // true（同一个对象）
```

## 语法格式

```java
public class Student {
    static String schoolName;  // 类变量
    int age;                   // 实例变量

    public static void test() { }  // 静态方法
}
```

## 踩坑记录

> [!warning] 静态方法不能访问非静态成员
> ```java
> public class Student {
>     int age;
>     public static void test() {
>         System.out.println(age);  // ❌ 编译错误
>         this.age;                 // ❌ 静态方法没有 this
>     }
> }
> ```
> **原因**：静态方法属于类，调用时可能还没有对象存在。

> [!warning] 静态变量被所有对象共享
> ```java
> s1.count = 1;
> s2.count = 2;
> // 如果 count 是 static → s1.count = 2（被 s2 覆盖了）
> // 如果 count 不是 static → s1.count = 1（独立）
> ```

## 实战案例

```java
// 统计创建了多少个对象
public class User {
    private static int count = 0;  // 类变量
    public User() { count++; }
    public static int getCount() { return count; }
}

new User(); new User();
System.out.println(User.getCount());  // 2
```

## 拓展延伸

→ [[11-继承多态抽象类接口|final 关键字]] · [[../../Java进阶之路/01-Java基础/static和final关键字]]

## 知识依赖图

先掌握 [[08-面向对象基础|OOP 基础]] → 再学本篇 → 延伸 [[11-继承多态抽象类接口|final]] · [[14-集合框架|工具类 Collections/Arrays]]

---

#技术/Java/OOP #技术/Java/基础 #笔记/考点复习
