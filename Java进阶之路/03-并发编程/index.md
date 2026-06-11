---
created: 2026-06-08
source: https://javabetter.cn/thread/
tags: [笔记/进阶之路, 技术/Java/并发, 导航]
---

# 🔁 并发编程

> 多线程、锁、线程池、JUC 工具类 — 并发编程核心知识体系

---

## 📑 内容目录

### 基础概念
- [[../02-Java进阶/并发编程核心知识|并发编程核心知识]] ⭐⭐⭐
  - 线程与进程 · 线程状态 · 创建线程三种方式

### 锁与同步
- synchronized 原理与锁升级（偏向锁/轻量级锁/重量级锁）
- volatile 与 JMM（可见性/禁止重排）
- AQS 抽象队列同步器原理

### 线程池
- 核心参数（corePoolSize/maxPoolSize/queue/handler）
- 工作流程 · 四种拒绝策略
- 禁止 Executors · 手动创建

### JUC 工具类
- CountDownLatch · CyclicBarrier · Semaphore
- ReentrantLock · ReentrantReadWriteLock
- ThreadLocal · Atomic 原子类

---

## 🔗 关联笔记

- [[../02-Java进阶/集合框架总览|集合框架总览]] — ConcurrentHashMap 底层
- [[../02-Java进阶/JVM核心知识|JVM核心知识]] — JMM 内存模型

---

#Java #并发 #线程 #锁
