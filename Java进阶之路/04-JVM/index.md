---
created: 2026-06-08
source: https://javabetter.cn/jvm/what-is-jvm.html
tags: [笔记/进阶之路, 技术/Java/JVM, 导航]
---

# ⚙️ JVM 核心知识

> JVM 内存结构、类加载机制、垃圾回收、调优排查 — Java 虚拟机完整知识体系

---

## 📑 内容目录

### 内存结构
- [[../02-Java进阶/JVM核心知识|JVM核心知识]] ⭐⭐⭐
  - 程序计数器 · 虚拟机栈 · 堆 · 方法区 · 本地方法栈
  - 堆分代（Eden/S0/S1/Old）
  - 对象创建过程 · 内存布局

### 类加载机制
- 加载 → 验证 → 准备 → 解析 → 初始化
- 双亲委派模型 · 破坏双亲委派

### 垃圾回收（GC）
- 可达性分析 · GC Roots · 四种引用
- GC 算法：标记-清除 / 复制 / 标记-整理
- 收集器：Serial / ParNew / CMS / G1 / ZGC

### 调优与排查
- JVM 参数：堆设置 / GC 日志 / OOM dump
- 排查工具：jps / jstat / jmap / jstack / Arthas / MAT

---

## 🔗 关联笔记

- [[../03-并发编程/index|并发编程]] — JMM 与 volatile 底层
- [[../02-Java进阶/集合框架总览|集合框架总览]] — 对象引用与 GC

---

#Java #JVM #GC #调优
