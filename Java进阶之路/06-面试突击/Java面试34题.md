---
title: Java 面试 34 题精华
created: 2026-06-08
source: https://javabetter.cn/interview/java-34.html
tags: [技术/Java/面试, 笔记/进阶之路]
---

# [[Java 面试]]34 题精华

## 一、Java 基础（10题）

**1. 面向对象三大特性**：封装、继承、多态  
**2. == vs equals()**：`==` 比地址，`equals()` 比内容（String/Integer 已重写）  
**3. String/StringBuilder/StringBuffer**：不可变/线程不安全/线程安全  
**4. 重载 vs 重写**：编译时多态 vs 运行时多态  
**5. 抽象类 vs 接口**：is-a vs like-a  
**6. HashSet 如何保证不重复**：hashCode() 定位桶 → equals() 判断  
**7. final**：类不可继承、方法不可重写、变量不可改引用  
**8. 异常体系**：Error / 受检异常 / RuntimeException  
**9. BIO/NIO/AIO**：同步阻塞 / 同步非阻塞 / 异步非阻塞  
**10. 序列化**：Serializable + transient + serialVersionUID

## 二、集合（6题）

**11. ArrayList 扩容**：默认 10，扩容 1.5 倍  
**12. HashMap 底层**：数组+链表+红黑树，高16位异或低16位，尾插  
**13. ConcurrentHashMap**：JDK7→分段锁，JDK8→CAS+synchronized  
**14. fail-fast 机制**：遍历时结构被修改→抛异常  
**15. Comparable vs Comparator**：内部 vs 外部比较器  
**16. LinkedHashMap LRU**：accessOrder=true 实现 LRU

## 三、并发（6题）

**17-18. 线程创建 + 状态**：Thread/Runnable/Callable/线程池  
**19. synchronized vs ReentrantLock**：自动/手动、可中断、公平性  
**20. volatile**：可见性+禁止重排，不保证原子性  
**21. 线程池参数**：corePoolSize → queue → maxPoolSize → 拒绝策略  
**22. ThreadLocal**：用完必须 remove()，否则内存泄漏

## 四、JVM（6题）

**23-24. 内存区域 + GC**：堆/栈/方法区/PC + 可达性分析/复制/标记整理  
**25-26. 类加载 + 双亲委派**：加载→验证→准备→解析→初始化  
**27. G1**：Region 分区，优先回收垃圾最多区域  
**28. OOM 场景**：堆/栈/方法区溢出

## 五、框架（6题）

**29. IoC/AOP**：控制反转 + 动态代理  
**30. Bean 生命周期**：实例化→赋值→初始化前→初始化→初始化后→销毁  
**31. 事务隔离级别**：READ UNCOMMITTED → SERIALIZABLE  
**32. MyBatis `#{}` vs `${}`**：预编译 vs 字符串拼接（防注入）  
**33. 索引失效**：左模糊/函数/隐式转换/NOT IN  
**34. 缓存穿透/击穿/雪崩**：布隆过滤器/互斥锁/随机过期

## 知识依赖图

先系统学习 [[../../Java SE 考点复习/00-目录索引|考点复习]] + [[../Java进阶之路|进阶之路]] → 再刷本篇 → 配合 [[../07-学习路线/Java学习路线一条龙|学习路线]] 查漏补缺

---

#技术/Java/面试 #笔记/进阶之路
