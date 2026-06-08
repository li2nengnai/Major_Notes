---
created: 2026-06-08
source: https://javabetter.cn/string/
tags: [java, string, 字符串]
优先级: ⭐⭐⭐
---

# String 深入理解

> 来源：[二哥的Java进阶之路](https://javabetter.cn/string/immutable.html)

---

## 一、String 不可变性

```java
public final class String {
    private final char value[];  // 不可变字符数组
    private int hash;            // 缓存 hashCode
}
```

**不可变的原因**：
- 类 `final`，不可继承
- `char[]` 用 `final` 修饰且不暴露
- 所有修改方法（substring、concat 等）都返回新 String

**不可变的好处**：
- 字符串常量池复用
- 线程安全（无需同步）
- hashCode 可缓存（HashMap 的 key）

---

## 二、字符串常量池

```
堆内存
├── 字符串常量池（JDK 7+）
│   ├── "java"      ← 字面量
│   ├── "hello"     ← 字面量
│   └── ...
├── String对象
└── 其他对象
```

```java
String s1 = "hello";               // 常量池
String s2 = "hello";               // 复用常量池对象
String s3 = new String("hello");   // 堆中新对象
String s4 = s3.intern();           // 返回常量池对象

s1 == s2  // true（同一个常量池对象）
s1 == s3  // false（堆对象 vs 常量池）
s1 == s4  // true（intern 返回常量池对象）
```

### intern() 方法
- 常量池中有 → 返回常量池引用
- 常量池中没有 → 放入常量池并返回引用

---

## 三、字符串拼接

```java
// 1. 常量拼接（编译期优化）
String s = "a" + "b" + "c";   // 编译为 "abc"

// 2. 变量拼接（底层 StringBuilder）
String s = a + b + c;         // 相当于 new StringBuilder().append(a).append(b).append(c).toString()

// 3. 循环拼接（⚠️ 注意性能）
String result = "";
for (String s : list) {
    result += s;   // 每次循环都 new StringBuilder
}
// ✅ 用 StringBuilder 替代
StringBuilder sb = new StringBuilder();
for (String s : list) { sb.append(s); }
String result = sb.toString();
```

---

## 四、StringBuilder vs StringBuffer

| 对比 | StringBuilder | StringBuffer |
|------|-------------|-------------|
| 线程安全 | ❌ | ✅（synchronized） |
| 性能 | 高 | 中 |
| 默认容量 | 16 | 16 |
| 扩容 | 2倍+2 | 2倍+2 |
| 推荐 | ✅ 单线程首选 | 多线程共享 |

---

## 五、常用方法

```java
// 比较
s.equals("hello");           // 比较值
s.equalsIgnoreCase("HELLO"); // 忽略大小写
s.compareTo("world");        // 字典序比较

// 查找
s.indexOf("l");              // 第一次出现位置
s.lastIndexOf("l");          // 最后一次出现位置
s.contains("el");            // 是否包含

// 截取
s.substring(1, 3);           // [1,3)
s.split(",");                // 分割

// 判断
s.isEmpty();                 // 是否空串
s.startsWith("he");          // 前缀
s.endsWith("lo");            // 后缀

// 转换
s.toUpperCase();             // 转大写
s.toLowerCase();             // 转小写
s.trim();                    // 去除首尾空格
s.replace('l', 'x');         // 替换字符
s.replaceAll("\\d+", "#");   // 正则替换
```

---

## 💡 面试高频题

1. **String 为什么不可变？** → final 类 + final char[] + 不暴露内部
2. **new String("abc") 创建了几个对象？** → 1个或2个（常量池有/无）
3. **字符串拼接用 + 还是 StringBuilder？** → 单次用 +，循环用 StringBuilder
4. **String、StringBuilder、StringBuffer 三者区别？** → 不可变/非线程安全/线程安全
5. **intern() 方法的作用？** → 从常量池获取字符串引用
