---
created: 2026-06-08
source: https://javabetter.cn/thread/
tags: [java, 并发, 线程, 多线程]
优先级: ⭐⭐⭐
---

# 并发编程核心知识

> 来源：[二哥的Java进阶之路](https://javabetter.cn/thread/why-need-thread.html)

---

## 一、线程与进程

| 对比 | 进程 | 线程 |
|------|------|------|
| 定义 | 程序一次执行过程 | 进程中独立执行路径 |
| 资源 | 独立内存空间 | 共享进程资源 |
| 切换开销 | 大 | 小 |
| 通信 | IPC（管道/消息队列） | 共享内存（直接读写） |

**Java 线程状态**：
```
NEW → RUNNABLE → BLOCKED/WAITING/TIMED_WAITING → TERMINATED
```

---

## 二、创建线程的三种方式

### 1. 继承 Thread
```java
class MyThread extends Thread {
    @Override
    public void run() { /* ... */ }
}
new MyThread().start();
```

### 2. 实现 Runnable（推荐）
```java
class MyTask implements Runnable {
    @Override
    public void run() { /* ... */ }
}
new Thread(new MyTask()).start();
```

### 3. 实现 Callable + FutureTask（带返回值）
```java
class MyCallable implements Callable<Integer> {
    @Override
    public Integer call() { return 42; }
}
FutureTask<Integer> ft = new FutureTask<>(new MyCallable());
new Thread(ft).start();
Integer result = ft.get();  // 获取返回结果
```

---

## 三、synchronized 原理与锁升级

### 使用方式
```java
// 1. 同步代码块（锁对象）
synchronized (this) { /* ... */ }

// 2. 同步方法（锁当前对象）
public synchronized void method() { /* ... */ }

// 3. 静态同步方法（锁 Class 对象）
public static synchronized void staticMethod() { /* ... */ }
```

### 锁升级过程（偏向锁 → 轻量级锁 → 重量级锁）

```
无锁 → 偏向锁 → 轻量级锁（CAS自旋） → 重量级锁（OS互斥量）
                                   ↑
                           始终不升级：从偏向锁直接到重量级（锁竞争激烈时）
```

| 锁状态 | 特点 | 适用场景 |
|--------|------|---------|
| 偏向锁 | 锁对象头记录线程ID | 单线程访问 |
| 轻量级锁 | CAS自旋，不阻塞 | 少量线程交替 |
| 重量级锁 | 线程阻塞，OS调度 | 多线程竞争激烈 |

**JDK 6 后锁优化**：
- 锁粗化：相邻同步块合并
- 锁消除：JIT 检测无竞争时删除锁
- 适应性自旋：自旋次数动态调整

---

## 四、volatile 与 JMM

### volatile 两个语义
1. **可见性**：写操作立即刷新到主存，读操作从主存读取
2. **禁止指令重排序**：通过内存屏障实现

### JMM（Java 内存模型）
```
线程A → 工作内存 → 主内存（共享变量） ← 线程B → 工作内存
         ↑                                    ↑
        read                                 write
```

**三大特性**：
| 特性 | 说明 | 实现 |
|------|------|------|
| 原子性 | 操作不可中断 | synchronized/Lock/atomic |
| 可见性 | 修改对其他线程可见 | volatile/synchronized |
| 有序性 | 禁止指令重排 | volatile/synchronized |

### happens-before 规则
- 程序次序规则
- volatile 变量规则（写 happens-before 读）
- 锁规则（解锁 happens-before 加锁）
- 传递性

---

## 五、AQS 原理（AbstractQueuedSynchronizer）

**核心**：CLH 变体队列 + volatile int state

```
        ┌─────────────────────────────────┐
        │   AbstractQueuedSynchronizer    │
        │   state (volatile int)          │
        │   head → Node ⇄ Node ⇄ Node     │
        └─────────────────────────────────┘
```

**模板方法**：
- `tryAcquire()` / `tryRelease()`（独占锁）
- `tryAcquireShared()` / `tryReleaseShared()`（共享锁）

**应用**：ReentrantLock、CountDownLatch、Semaphore、ThreadPoolExecutor

---

## 六、线程池

### 核心参数
```java
ThreadPoolExecutor executor = new ThreadPoolExecutor(
    corePoolSize,       // 核心线程数
    maximumPoolSize,    // 最大线程数
    keepAliveTime,      // 空闲线程存活时间
    TimeUnit.SECONDS,
    new LinkedBlockingQueue<>(queueSize),  // 任务队列
    Executors.defaultThreadFactory(),      // 线程工厂
    new ThreadPoolExecutor.AbortPolicy()   // 拒绝策略
);
```

### 线程池工作流程
```
提交任务
   ↓
核心线程未满？ → 是 → 创建核心线程执行
   ↓ 否
任务队列未满？ → 是 → 入队等待
   ↓ 否
最大线程未满？ → 是 → 创建临时线程执行
   ↓ 否
执行拒绝策略
```

### 四种拒绝策略
| 策略 | 行为 |
|------|------|
| AbortPolicy（默认） | 抛出 RejectedExecutionException |
| CallerRunsPolicy | 调用者线程执行 |
| DiscardPolicy | 直接丢弃 |
| DiscardOldestPolicy | 丢弃最老任务 |

### 禁止使用 Executors 创建线程池！
```java
// ❌ 有风险（队列无界，可能 OOM）
Executors.newFixedThreadPool(10);   // LinkedBlockingQueue 无界
Executors.newCachedThreadPool();    // 最大线程数 Integer.MAX_VALUE

// ✅ 手动创建，明确参数
new ThreadPoolExecutor(10, 20, 60L, TimeUnit.SECONDS,
    new ArrayBlockingQueue<>(100), new ThreadPoolExecutor.AbortPolicy());
```

---

## 七、JUC 常用工具类

| 类 | 作用 |
|------|------|
| `CountDownLatch` | 一组线程等待另一组线程完成 |
| `CyclicBarrier` | 一组线程互相等待到齐 |
| `Semaphore` | 信号量，控制并发数量 |
| `ReentrantLock` | 可重入锁，比 synchronized 更灵活 |
| `ReentrantReadWriteLock` | 读写分离锁 |
| `ThreadLocal` | 线程本地变量 |
| `atomic` 包 | CAS 实现原子操作 |

---

## 💡 面试高频题

1. **synchronized 和 Lock 的区别？** → 自动/手动、是否可中断、公平性
2. **volatile 能保证原子性吗？** → 不能（如 i++ 非原子操作）
3. **线程池核心线程数怎么设置？** → CPU 密集型 N+1，IO 密集型 2N
4. **ThreadLocal 内存泄漏？** → key 弱引用，value 强引用，需手动 remove()
5. **CAS 的 ABA 问题？** → AtomicStampedReference 版本号解决
