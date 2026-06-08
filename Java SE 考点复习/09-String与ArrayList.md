---
created: 2026-06-08
topic: Java SE 考点复习 - String 与 ArrayList
tags: [Java, 编程笔记, 考点复习, String, ArrayList]
---

# String 与 ArrayList

## 📌 课程模块

pk008 · String 常用方法/字符串池/ArrayList 集合 · **高频考点**

## 💻 核心代码示例

```java
// 1. String 两种创建方式（内存位置不同！）
public class StringDemo1 {
    public static void main(String[] args) {
        // 方式1：直接双引号（字符串常量池）
        String name = "IT666";

        // 方式2：new String（堆内存新对象）
        String rs1 = new String();
        String rs2 = new String("itheima");
        char[] chars = {'a', '三', '十'};
        String rs3 = new String(chars);        // "三十"
        byte[] bytes = {97, 98, 99};
        String rs4 = new String(bytes);        // "abc"
    }
}
```

```java
// 2. String 常用方法（需要熟记！）
public class StringDemo2 {
    public static void main(String[] args) {
        String s = "Java";
        System.out.println(s.length());              // 4

        char c = s.charAt(1);                        // 'a'
        // 遍历方式1
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
        }
        // 遍历方式2
        char[] chars = s.toCharArray();

        // ⭐ equals 比较内容（== 比较地址）
        String s1 = new String("hello");
        String s2 = new String("hello");
        System.out.println(s1 == s2);                // false（地址不同）
        System.out.println(s1.equals(s2));           // true（内容相同）

        System.out.println("ABC".equalsIgnoreCase("abc")); // true

        // substring 包前不包后
        String s3 = "Java是最好的语言";
        String rs = s3.substring(0, 8);              // "Java是最好的"
        String rs2 = s3.substring(5);                // 从索引5到结尾

        String info = "垃圾电影";
        System.out.println(info.replace("垃圾", "**"));  // "**电影"

        System.out.println("Java".contains("Java")); // true
        System.out.println("张三丰".startsWith("张"));// true

        String names = "张无忌,周芷若,赵敏";
        String[] arr = names.split(",");             // [张无忌, 周芷若, 赵敏]
    }
}
```

```java
// 3. ArrayList 集合（可变数组）
import java.util.ArrayList;
public class ArrayListDemo1 {
    public static void main(String[] args) {
        // 创建集合（泛型：指定元素类型）
        ArrayList<String> list = new ArrayList<>();

        // 添加元素
        list.add("极客");                            // 末尾添加
        list.add("Java");
        list.add(1, "MySQL");                       // 指定位置插入

        System.out.println(list);                   // [极客, MySQL, Java]

        // 获取元素
        String rs = list.get(1);                     // "MySQL"

        // 获取大小
        System.out.println(list.size());             // 3

        // 删除
        list.remove(1);                              // 按索引删除，返回被删元素
        list.remove("Java");                         // 按元素删除（只删第一个匹配）

        // 修改
        list.set(1, "极客大牛");                      // 替换，返回旧值

        // 遍历
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
    }
}
```

## 🧩 知识点拆解

### 知识点1：String 两种创建方式的区别

- **是什么**：`"直接量"` 存储在字符串常量池（重复使用），`new String()` 在堆中创建新对象
- **老师强调的要点**（**必考！**）：
  - `"直接量"` 形式：内容相同时复用常量池中已有对象，`==` 为 true
  - `new String()`：每次创建新对象，即使内容相同 `==` 也是 false
  - 字符串内容比较**必须用 `equals()`**，不能用 `==`

### 知识点2：String 的不可变性

- **是什么**：String 对象创建后内容不可变，每次修改都会创建新对象
- **老师强调的要点**：
  - `replace()`、`substring()` 等方法都返回新字符串，原字符串不变
  - 字符串频繁拼接 → 产生大量垃圾对象，性能差 → 改用 `StringBuilder`

### 知识点3：ArrayList 集合

- **是什么**：长度可变的数组，存储引用类型数据
- **在代码中的作用**：替代固定长度数组，灵活添加/删除元素
- **老师强调的要点**：
  - 泛型 `<>` 中**不能放基本类型**：`ArrayList<int>` ❌ → 要放包装类 `ArrayList<Integer>` ✅
  - `add(index, element)` 插入时后续元素后移
  - `remove(Object)` 删除的是**第一次出现的**该元素
  - 遍历时增删元素注意索引变化

## ⚠️ 常见考题 / 易错点

1. **选择题——String 比较**：
   ```java
   String s1 = "abc";
   String s2 = "abc";
   String s3 = new String("abc");
   System.out.println(s1 == s2);       // true（常量池）
   System.out.println(s1 == s3);       // false（堆中不同对象）
   System.out.println(s1.equals(s3));  // true（内容相同）
   ```

2. **选择题——String 不可变性**：
   ```java
   String s = "abc";
   s.toUpperCase();
   System.out.println(s);             // "abc"（原字符串不变，需要重新赋值）
   ```

3. **选择题——ArrayList 泛型**：以下哪个正确？
   - `ArrayList<int>` ❌
   - `ArrayList<Integer>` ✅
   - `ArrayList<Object>` ✅

4. **编程题——字符串处理**：统计字符出现次数、手机号屏蔽、敏感词替换

---

#Java #编程笔记 #考点复习 #String #ArrayList
