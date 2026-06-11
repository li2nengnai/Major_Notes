---
title: static 与 final 关键字
created: 2026-06-08
source: https://javabetter.cn/oo/static.html
tags: [技术/Java/OOP, 技术/Java/基础, 笔记/进阶之路]
---

# [[static]] 与 [[final]] 关键字

## 核心概念

### static

属于**类**，不属于对象，类加载时初始化，被所有实例共享。

```java
public class StaticExample {
    public static int count = 0;    // 类变量
    public static void inc() { }    // 静态方法
    static { }                      // 静态代码块（类加载时执行一次）
}
```

**静态方法限制**：不能访问非静态成员，不能使用 `this`，不能重写（只有方法隐藏）。

### final

```java
final class A { }             // 类：不可继承（如 String）
final void test() { }         // 方法：不可重写
final int MAX = 100;          // 变量：值不可变
final List<String> list = new ArrayList<>();
list.add("a");                // ✅ 对象内容可变
// list = new ArrayList<>();  // ❌ 引用不可变
```

### 编译期常量 vs 运行期常量

```java
final int A = 10;                     // 编译期（编译时替换）
final int B = new Random().nextInt();  // 运行期
```

## 踩坑记录

> [!warning] final 和 finally 和 finalize 的区别
> - `final`：修饰符
> - `finally`：异常处理的代码块
> - `finalize`：GC 回收前调用的方法（已废弃）

> [!warning] static 方法不能访问非 static 成员
> 因为静态方法调用时可能还没有对象存在。

## 知识依赖图

先掌握 [[../../Java SE 考点复习/08-面向对象基础|OOP]] → 再学本篇 → 延伸 [[../../Java SE 考点复习/10-static关键字|static 考点]]

---

#技术/Java/OOP #技术/Java/基础 #笔记/进阶之路
