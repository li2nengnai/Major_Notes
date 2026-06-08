---
created: 2026-06-08
source: https://javabetter.cn/redis/
tags: [java, redis, 缓存]
优先级: ⭐⭐⭐
---

# Redis 核心知识点

> 来源：[二哥的Java进阶之路](https://javabetter.cn/redis/)

---

## 一、Redis 数据结构

| 类型 | 底层实现 | 使用场景 |
|------|---------|---------|
| **String** | SDS（动态字符串） | 缓存、计数器、分布式锁 |
| **List** | 双向链表/压缩列表 | 消息队列、最新列表 |
| **Set** | 哈希表+整数数组 | 去重、共同好友 |
| **ZSet** | 跳表+哈希表 | 排行榜、延迟队列 |
| **Hash** | 压缩列表/哈希表 | 对象存储 |

### 底层实现

**SDS（Simple Dynamic String）**
```c
struct sdshdr {
    int len;      // 已用长度
    int free;     // 空闲空间
    char buf[];   // 字节数组
};
// 相比 C 字符串：O(1) 获取长度、空间预分配、惰性释放、二进制安全
```

**跳表（SkipList）** — ZSet 的排序核心
```
Level 3: 头 ──────────────────────→ 尾
Level 2: 头 ──────→ 30 ──────────→ 尾
Level 1: 头 ──→ 10 → 20 → 30 → 40 → 尾
```
多层链表，上层是下层索引，查询 O(log N)

---

## 二、持久化

| 对比 | RDB | AOF |
|------|-----|-----|
| 原理 | 快照全量数据 | 追加写命令 |
| 文件大小 | 小 | 大 |
| 恢复速度 | 快 | 慢 |
| 数据安全 | 丢最后一次快照 | 根据策略（always/everysec） |
| 对性能影响 | 大（fork 子进程） | 小 |
| 适用场景 | 备份/容灾 | 数据安全优先 |

**推荐配置**：RDB + AOF 同时开启

**AOF 重写**：将多个写命令合并，减少文件大小

---

## 三、缓存三大问题

### 3.1 缓存穿透
**现象**：查询不存在的数据，缓存和数据库都没有，请求直接打到 DB

**解决方案**：
- 布隆过滤器：判断 key 不存在直接拦截
- 缓存空值：null 也缓存（设置较短过期时间）

### 3.2 缓存击穿
**现象**：热点 key 过期，大量请求同时打到 DB

**解决方案**：
- 互斥锁：只让一个请求去查 DB，其他等待
- 逻辑过期：不设置过期时间，值中存逻辑过期时间

### 3.3 缓存雪崩
**现象**：大量 key 同时过期，或 Redis 宕机，请求全部打到 DB

**解决方案**：
- 过期时间加随机值
- 多级缓存（本地缓存 + Redis）
- Redis 集群（主从 + Sentinel）
- 熔断降级

---

## 四、Redis 分布式锁

```java
// 加锁
String key = "lock:order:" + orderId;
Boolean locked = redisTemplate.opsForValue()
    .setIfAbsent(key, threadId, 30, TimeUnit.SECONDS);

// 解锁（Lua 脚本保证原子性）
String lua = "if redis.call('get',KEYS[1]) == ARGV[1] " +
             "then return redis.call('del',KEYS[1]) " +
             "else return 0 end";
redisTemplate.execute(new DefaultRedisScript<>(lua, Long.class), 
    Arrays.asList(key), threadId);
```

**关键点**：
- 设置过期时间（防止死锁）
- value 存线程唯一标识（防止误删别人的锁）
- Lua 脚本保证原子性
- Redisson 提供可重入锁

---

## 五、Redis 过期策略与淘汰机制

### 过期策略
- **定期删除**：每隔 100ms 随机抽查一批有过期时间的 key
- **惰性删除**：访问时检查是否过期

### 内存淘汰机制（maxmemory-policy）
| 策略 | 说明 |
|------|------|
| noeviction（默认） | 不淘汰，写返回错误 |
| allkeys-lru | 淘汰最近最少使用的 key |
| volatile-lru | 淘汰设置了过期时间的 key 中最近最少使用的 |
| allkeys-lfu | 淘汰使用频率最低的 key |
| volatile-lfu | 淘汰设置了过期时间的 key 中频率最低的 |
| allkeys-random | 随机淘汰 |
| volatile-ttl | 淘汰即将过期的 key |

---

## 💡 面试高频题

1. **Redis 为什么快？** → 内存操作、单线程（避免上下文切换/竞争）、IO 多路复用、高效数据结构
2. **Redis 单线程为什么能处理高并发？** → 基于内存 + IO 多路复用（epoll）
3. **缓存和数据库一致性怎么保证？** → 先更新数据库，再删除缓存（Cache Aside Pattern）
4. **Redis 主从/哨兵/集群区别？** → 主从复制、Sentinel 高可用、Cluster 分片
5. **跳表 vs 红黑树？** → 跳表范围查询友好，实现简单
