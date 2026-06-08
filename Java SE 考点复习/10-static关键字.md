---
created: 2026-06-08
topic: Java SE 考点复习 - static 关键字
tags: [Java, 编程笔记, 考点复习, static]
---

# static 关键字

## 📌 课程模块

pk009 · 静态变量/静态方法/工具类/静态代码块/单例模式 · **高频考点**

## 💻 核心代码示例

```java
// 1. static 修饰成员变量：类变量 vs 实例变量
public class Student {
    static String name;                // 类变量：属于类，所有对象共享
    int age;                           // 实例变量：属于对象，每个对象独立
}

public class Test {
    public static void main(String[] args) {
        // 类变量：推荐类名访问
        Student.name = "袁华";

        Student s1 = new Student();
        s1.name = "马冬梅";              // 修改了类变量的值（所有对象共享）
        s1.age = 23;

        Student s2 = new Student();
        s2.name = "秋雅";                // 再次修改类变量

        System.out.println(s1.name);     // "秋雅"（类变量共享，改一个全改）
        System.out.println(Student.name);// "秋雅"（类名访问）
        System.out.println(s1.age);      // 23（实例变量独立）
        System.out.println(s2.age);      // 18（不同对象不同）
        // System.out.println(Student.age); // ❌ 报错！实例变量不能通过类名访问
    }
}
```

```java
// 2. 工具类设计（所有方法静态，构造器私有）
public class MyUtil {
    // 构造器私有：不允许外部创建对象
    private MyUtil() { }

    // 工具方法全部静态
    public static void printHello() {
        System.out.println("Hello");
    }
}

// 直接类名调用，无需创建对象
MyUtil.printHello();
```

```java
// 3. 单例模式（饿汉式）
public class A {
    // 2. 类变量记住唯一实例
    private static A a = new A();

    // 1. 构造器私有
    private A() { }

    // 3. 公开静态方法获取实例
    public static A getObject() {
        return a;
    }
}

class CC {
    void dd() {
        A a1 = A.getObject();
        A a2 = A.getObject();
        System.out.println(a1 == a2);     // true（同一个对象）
    }
}
```

## 🧩 知识点拆解

### 知识点1：类变量 vs 实例变量

- **是什么**：
  - 类变量（`static`）：属于类，所有对象共享一份数据，类加载时分配内存
  - 实例变量（无 `static`）：属于对象，每个对象独立一份，new 时分配内存
- **老师强调的要点**（**必考！**）：
  - 类变量推荐用 `类名.变量名` 访问（`Student.name`）
  - 类变量被所有对象共享，任意对象修改后，其他对象都受影响
  - 实例变量不能用类名访问（编译报错）

### 知识点2：工具类

- **是什么**：所有方法都是静态的实用类，构造器私有，不允许创建对象
- **在代码中的作用**：提供通用工具方法（如 `Arrays`、`Collections`）
- **老师强调的要点**：
  - 构造器私有确保外部不能 new 对象
  - 所有方法用 `static` 修饰，直接用类名调用

### 知识点3：单例模式

- **是什么**：确保一个类只有一个对象
- **三要素**：私有构造器 + 静态私有的实例变量 + 静态公开的获取方法
- **老师强调的要点**：
  - 饿汉式：类加载时就创建实例（线程安全）
  - 懒汉式：首次调用 `getObject()` 时才创建（需考虑线程安全问题）

### 知识点4：静态代码块

- **是什么**：`static { }` 在类加载时执行，且**只执行一次**
- **用途**：初始化静态成员变量、加载配置文件等

## ⚠️ 常见考题 / 易错点

1. **选择题——static 变量共享**：
   ```java
   Student s1 = new Student();
   Student s2 = new Student();
   s1.count = 1; s2.count = 2;
   // 如果 count 是 static：s1.count = ? → 2（共享）
   // 如果 count 是非 static：s1.count = ? → 1（独立）
   ```

2. **填空题——静态方法限制**：静态方法中**不能直接访问**非静态成员，**不能使用 this**（因为没有当前对象）

3. **选择题——main 方法为什么是 static**：JVM 调用 main 方法时无需创建对象

4. **编程题——工具类设计**：构造器私有 + 所有方法静态

---

#Java #编程笔记 #考点复习 #static #单例模式
