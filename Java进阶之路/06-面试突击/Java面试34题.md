---
created: 2026-06-08
source: https://javabetter.cn/interview/java-34.html
tags: [java, 面试, 高频]
优先级: ⭐⭐⭐
---

# Java 面试 34 题（精华版）

> 来源：[二哥的Java进阶之路](https://javabetter.cn/interview/java-34.html)

---

## 一、Java 基础（10题）

### 1. 面向对象三大特性
**封装**：隐藏实现，提供访问接口  
**继承**：子类复用父类，`extends` 单继承  
**多态**：父类引用指向子类对象，方法重写实现

### 2. == 和 equals() 区别
- `==`：基本类型比值，引用类型比**地址**
- `equals()`：默认比地址，String/Integer 等重写后比值

### 3. String、StringBuilder、StringBuffer
| 类型 | 可变 | 线程安全 | 性能 |
|------|------|---------|------|
| String | ❌ | ✅ | 低（拼接创建新对象） |
| StringBuilder | ✅ | ❌ | 高 |
| StringBuffer | ✅ | ✅（synchronized） | 中 |

### 4. 重载和重写的区别
- **重载**：同类中方法名相同参数不同（编译时多态）
- **重写**：子类覆盖父类方法（运行时多态）

### 5. 抽象类和接口的区别
- 抽象类：is-a，可有构造方法/成员变量/普通方法
- 接口：like-a，JDK8 后可 default/static 方法

### 6. HashSet 如何保证不重复
- 基于 HashMap，存为 key
- hashCode() 定位桶 → equals() 判断是否重复

### 7. final 关键字
- 类：不可继承
- 方法：不可重写
- 变量：不可改引用（基本类型值，引用类型地址）

### 8. 异常体系
```
Throwable
├── Error（不可恢复，OOM、StackOverflow）
└── Exception
    ├── RuntimeException（非受检，NPE、下标越界）
    └── 受检异常（IOException、SQLException）
```

### 9. BIO/NIO/AIO
| 类型 | 同步/异步 | 阻塞/非阻塞 | 场景 |
|------|----------|------------|------|
| BIO | 同步 | 阻塞 | 连接数少 |
| NIO | 同步 | 非阻塞 | 连接数多&短 |
| AIO | 异步 | 非阻塞 | 连接数多&长 |

### 10. 序列化
- `Serializable` 接口（标记接口）
- `transient` 关键字阻止序列化
- `serialVersionUID` 保证版本兼容

---

## 二、集合框架（6题）

### 11. ArrayList 扩容机制
默认容量10，扩容1.5倍，`Arrays.copyOf()` 拷贝

### 12. HashMap 底层原理
JDK 8：数组+链表+红黑树，hash 算法（高16位异或低16位），尾插法

### 13. ConcurrentHashMap 如何保证线程安全
- JDK 7：Segment 分段锁
- JDK 8：CAS + synchronized（锁数组单个位置）

### 14. 迭代器 fail-fast 机制
- 迭代过程中结构被修改 → 抛出 `ConcurrentModificationException`
- modCount 计数器检测

### 15. Comparable 和 Comparator 区别
- `Comparable`：内部比较器，`compareTo()` 自然排序
- `Comparator`：外部比较器，`compare()` 定制排序

### 16. LinkedHashMap 如何实现 LRU
继承 HashMap + 双向链表维护顺序，`accessOrder=true` 实现 LRU

---

## 三、并发编程（6题）

### 17. 创建线程方式
Thread、Runnable、Callable+FutureTask、线程池

### 18. 线程状态
NEW → RUNNABLE → WAITING/TIMED_WAITING/BLOCKED → TERMINATED

### 19. synchronized 和 ReentrantLock
| 对比 | synchronized | ReentrantLock |
|------|-------------|---------------|
| 使用 | 自动加解锁 | 手动 lock/unlock |
| 中断 | ❌ | ✅ lockInterruptibly() |
| 超时 | ❌ | ✅ tryLock() |
| 公平 | 非公平 | 可设公平 |
| 条件 | 单个 | 多个 Condition |

### 20. volatile 作用
可见性（写刷新主存）+ 禁止指令重排 | **不能保证原子性**

### 21. 线程池参数
corePoolSize, maxPoolSize, keepAliveTime, workQueue, threadFactory, handler

### 22. ThreadLocal 原理
每个线程维护 ThreadLocalMap，key 弱引用 ThreadLocal，**用完必须 remove()**

---

## 四、JVM（6题）

### 23-24. 内存区域 / GC
堆、栈、方法区、PC、本地方法栈 | 可达性分析、GC Root、复制/标记清除/标记整理

### 25. 类加载过程
加载→验证→准备→解析→初始化

### 26. 双亲委派模型
Bootstrap → Extension → Application → 自定义，避免重复加载+保证安全

### 27. G1 收集器
Region 分区、优先回收垃圾最多区域、可预测停顿时间

### 28. OOM 常见场景
堆溢出（对象太多）、栈溢出（递归太深）、方法区溢出（动态生成类）

---

## 五、框架（6题）

### 29. IoC / AOP
IoC：控制反转，容器管理对象 | AOP：面向切面，动态代理

### 30. Bean 生命周期
实例化→属性赋值→初始化前→初始化→初始化后→使用→销毁

### 31. Spring 事务隔离级别
Default/ReadUncommitted/ReadCommitted/RepeatableRead/Serializable

### 32. MyBatis #{} 和 ${} 区别
- `#{}`：预编译，防 SQL 注入 ✅
- `${}`：直接拼接字符串 ❌

### 33. MySQL 索引失效场景
左模糊、函数操作、隐式转换、NOT IN、!=、联合索引跳过最左列

### 34. Redis 缓存穿透/击穿/雪崩
- 穿透：布隆过滤器/缓存空值
- 击穿：互斥锁/逻辑过期
- 雪崩：随机过期/多级缓存/集群
