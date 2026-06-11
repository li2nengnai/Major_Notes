---
title: JVM 核心知识
created: 2026-06-08
source: https://javabetter.cn/jvm/what-is-jvm.html
tags: [技术/Java/JVM, 技术/Java/进阶, 笔记/进阶之路]
---

# [[JVM]] 核心知识

## 核心概念

### 内存结构（运行时数据区）

```
线程私有：程序计数器 + 虚拟机栈 + 本地方法栈
线程共享：堆（Heap）+ 方法区（Metaspace）
```

**堆分代（默认 8:1:1）**：
```
Heap
├── Young Generation
│   ├── Eden（8/10）
│   ├── S0（1/10）
│   └── S1（1/10）
└── Old Generation
```

### 类加载过程

```
加载 → 验证 → 准备 → 解析 → 初始化
```

### 双亲委派模型

```
Bootstrap ClassLoader → Extension ClassLoader → Application ClassLoader → 自定义
```

加载类时先委托父加载器，父加载器加载不了才自己加载。**好处**：避免重复加载，防止核心 API 被篡改。

### 垃圾回收（GC）

**判断存活**：可达性分析（从 GC Roots 出发）

**四种引用**：

| 类型 | 回收时机 | 用途 |
|------|---------|------|
| 强引用 | 永不 | `new Object()` |
| 软引用 | OOM 前 | 缓存 |
| 弱引用 | 下次 GC | ThreadLocal |
| 虚引用 | 随时 | 管理直接内存 |

**GC 算法**：标记-清除、复制（新生代）、标记-整理（老年代）

**收集器**：Serial → ParNew → CMS → **G1（JDK9+默认）** → ZGC

### JVM 调优常用参数

```bash
-Xms4g -Xmx4g              # 堆大小（建议一致，避免扩容）
-Xmn2g                     # 年轻代
-XX:+UseG1GC               # G1 收集器
-XX:+HeapDumpOnOutOfMemoryError  # OOM 时 dump
```

## 踩坑记录

> [!warning] 线上 OOM 排查步骤
> 1. `jps` 找到进程 ID
> 2. `jmap -dump:format=b,file=heap.hprof <pid>` 导出堆
> 3. 用 MAT 或 jhat 分析
> 4. 定位大对象或泄漏点

> [!warning] CPU 100% 排查
> 1. `top -H` 找到 CPU 高的线程
> 2. `jstack <pid>` 导出线程栈
> 3. 找到对应线程的 nid（十六进制），看它在执行什么代码

## 知识依赖图

先掌握 [[../../Java SE 考点复习/08-面向对象基础|OOP]] · [[../01-Java基础/数据类型|数据类型]] → 再学本篇 → 延伸 [[../04-JVM/index|JVM 体系]]

---

#技术/Java/JVM #技术/Java/进阶 #笔记/进阶之路
