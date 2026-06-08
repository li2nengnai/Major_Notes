---
created: 2026-06-08
source: https://javabetter.cn/oo/static.html
tags: [java, 基础, static, final]
优先级: ⭐⭐⭐
---

# static 与 final 关键字

> 来源：[二哥的Java进阶之路](https://javabetter.cn/oo/static.html)

---

## static 关键字

### 核心特点
- static 成员属于**类**，而不属于对象
- 在类加载时初始化，优先于对象存在
- 被所有实例共享

### 使用场景

```java
public class StaticExample {
    // 1. 静态变量
    public static int count = 0;
    
    // 2. 静态方法（不能访问非静态成员）
    public static void increment() { 
        count++; 
        // this.count ❌ 没有 this 引用
    }
    
    // 3. 静态代码块（类加载时执行一次）
    static {
        System.out.println("类加载时执行");
    }
    
    // 4. 静态内部类（不依赖外部类实例）
    public static class Inner {
        public void print() { /* ... */ }
    }
}

// 5. 静态导入（Java 5+）
import static java.lang.Math.*;
// 直接使用 abs(), max() 等
```

### 静态方法 vs 实例方法
| 对比 | 静态方法 | 实例方法 |
|------|---------|---------|
| 归属 | 类 | 对象 |
| 调用 | 类名.方法名() | 对象.方法名() |
| this 引用 | ❌ | ✅ |
| 访问静态成员 | ✅ | ✅ |
| 访问非静态成员 | ❌ | ✅ |
| 重写（多态） | ❌（方法隐藏） | ✅ |

> **静态方法不能重写**：静态方法属于类，不存在多态。子类定义同名静态方法叫"方法隐藏"。

---

## final 关键字

### 三种用法

```java
// 1. final 类：不能被继承
public final class String { /* ... */ }

// 2. final 方法：不能被重写
public class Parent {
    public final void cannotOverride() { /* ... */ }
}

// 3. final 变量：值不可变
final int CONSTANT = 100;        // 基本类型：值不可变
final List<String> list = new ArrayList<>();
list.add("hello");               // ✅ 对象内容可变
// list = new ArrayList<>();     // ❌ 引用不可变（不可重新赋值）
```

### final 变量的初始化时机
- **类变量**（static final）：声明时或静态代码块
- **实例变量**（final）：声明时、构造方法或代码块
- **局部变量**（final）：使用前赋值一次即可

### 编译期常量 vs 运行期常量
```java
final int A = 10;              // 编译期常量（编译时替换为字面量）
final int B = new Random().nextInt(); // 运行期常量
```

---

## 💡 常见面试陷阱

1. **静态方法能不能访问非静态成员？** → 不能，静态方法没有 this 引用
2. **final 和 finally 和 finalize 区别？** → final 修饰符、finally 异常处理、finalize 垃圾回收前调用
3. **static 方法能重写吗？** → 不能，只有方法隐藏
4. **final 修饰的引用能改变对象内容吗？** → 可以，final 限制的是引用不可变
