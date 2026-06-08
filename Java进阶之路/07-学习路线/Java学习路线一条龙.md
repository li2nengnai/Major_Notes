---
created: 2026-06-08
source: https://javabetter.cn/xuexiluxian/java/yitiaolong.html
tags: [java, 学习路线, roadmap]
优先级: ⭐⭐⭐
---

# Java 学习路线一条龙（2026版）

> 来源：[二哥的Java进阶之路](https://javabetter.cn/xuexiluxian/java/yitiaolong.html)

---

## 🗺️ 总览

```
第一阶段：Java 基础
    ↓
第二阶段：数据库（MySQL + Redis）
    ↓
第三阶段：Java 进阶（并发 + JVM）
    ↓
第四阶段：企业级框架（Spring Boot 等）
    ↓
第五阶段：中间件（MQ、ES、微服务）
    ↓
第六阶段：实战项目 + 面试准备
```

---

## 第一阶段：Java 基础

### 1.1 Java 概述
- 什么是 Java、JDK/JRE/JVM 关系
- 安装 JDK、配置环境变量
- 第一个 Hello World 程序
- IDE 推荐：IntelliJ IDEA

### 1.2 Java 语法基础
- [[../01-Java基础/数据类型|基本数据类型]]：byte/short/int/long/float/double/boolean/char
- [[../01-Java基础/流程控制|流程控制]]：if-else、switch、for、while
- 运算符：算术、关系、逻辑、位运算
- 类型转换：自动转换、强制转换

### 1.3 面向对象
- [[../01-Java基础/面向对象三大特性|三大特性]]：封装、继承、多态
- 类与对象、构造方法
- `static`、`final`、`this`、`super` 关键字
- 抽象类与接口
- 内部类
- 包与访问控制（public/protected/default/private）

### 1.4 数组与字符串
- 数组声明与初始化
- String 不可变性
- StringBuilder 与 StringBuffer
- 字符串常量池
- equals() 与 hashCode()

### 1.5 集合框架
- [[../02-Java进阶/集合框架总览|集合框架体系]]：Collection 和 Map 两大分支
- ArrayList vs LinkedList
- HashMap 原理（数组+链表+红黑树、扩容机制）
- TreeMap、LinkedHashMap、ConcurrentHashMap
- Comparable 与 Comparator

### 1.6 异常处理
- try-catch-finally
- 受检异常 vs 非受检异常
- try-with-resources
- 自定义异常

---

## 第二阶段：数据库

### 2.1 MySQL
- 数据库设计、数据类型
- CRUD、多表查询、子查询
- [[../05-企业级开发/MySQL索引与优化|索引原理]]（B+ 树、聚簇索引）
- 事务（ACID、隔离级别）
- MVCC 原理
- SQL 优化与慢查询

### 2.2 Redis
- 五大数据类型：String/List/Set/ZSet/Hash
- 持久化：RDB vs AOF
- 缓存穿透/击穿/雪崩
- Redis 分布式锁

---

## 第三阶段：Java 进阶

### 3.1 并发编程
- [[../02-Java进阶/并发编程核心知识|线程与进程]]、线程状态、创建方式
- synchronized 原理、锁升级
- volatile 与 JMM
- AQS 抽象队列同步器
- 线程池：corePoolSize、workQueue、拒绝策略
- JUC 工具类：CountDownLatch、CyclicBarrier、Semaphore
- ConcurrentHashMap 原理
- ThreadLocal

### 3.2 JVM
- [[../02-Java进阶/JVM核心知识|JVM 内存结构]]：堆、栈、方法区、PC 寄存器
- 类加载机制：双亲委派模型
- GC 算法：标记-清除、复制、标记-整理
- 垃圾收集器：G1、ZGC、CMS
- 内存泄漏与 OOM 排查

### 3.3 Java 8+
- Lambda 表达式
- Stream API
- Optional
- 函数式接口

---

## 第四阶段：企业级框架

- [[../05-企业级开发/Spring Boot核心|Spring Boot]]：IoC、AOP、自动配置
- Spring MVC 工作流程
- MyBatis 使用与原理
- 事务管理 @Transactional

---

## 第五阶段：中间件

- MQ：Kafka、RabbitMQ
- Elasticsearch 搜索引擎
- Docker 容器化部署
- Nginx 反向代理
- 微服务与分布式

---

## 第六阶段：实战项目 + 面试

- [[../06-面试突击/index|面试突击]]：刷高频面试题
- 技术派项目实践
- LeetCode 算法刷题

---

## 📚 推荐书单

| 阶段 | 书名 | 理由 |
|------|------|------|
| 入门 | 《Head First Java》 | 图文并茂，适合零基础 |
| 基础 | 《Java核心技术 卷I》 | 全面系统的 Java 教程 |
| 进阶 | 《Java编程思想》 | 深入理解 Java 设计思想 |
| 并发 | 《Java并发编程的艺术》 | 并发必读 |
| JVM | 《深入理解Java虚拟机》 | JVM 圣经 |
| 数据库 | 《高性能MySQL》 | MySQL 进阶必备 |
| Spring | 《Spring实战》 | Spring 入门首选 |
