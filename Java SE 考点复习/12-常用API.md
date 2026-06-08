---
created: 2026-06-08
topic: Java SE 考点复习 - 常用 API
tags: [Java, 编程笔记, 考点复习, API, 包装类, BigDecimal, Date, StringBuilder]
---

# 常用 API（包装类 / 时间 / Math / BigDecimal / Arrays）

## 📌 课程模块

pk011-pk012 · 包装类/StringBuilder/枚举/时间/BigDecimal/Arrays · **理解考点**

## 💻 核心代码示例

```java
// 1. 包装类 Integer（自动装箱拆箱）
public class Test {
    public static void main(String[] args) {
        // 自动装箱：基本类型 → 包装类对象
        Integer a = 12;                   // 等价于 Integer.valueOf(12)

        // 自动拆箱：包装类 → 基本类型
        int b = a;                        // 等价于 a.intValue()

        // 泛型必须用包装类
        ArrayList<Integer> list = new ArrayList<>();
        list.add(12);                     // 自动装箱
        int rs = list.get(1);             // 自动拆箱

        // 字符串 ↔ 基本类型转换（重要！）
        String ageStr = "29";
        int age1 = Integer.parseInt(ageStr);     // 推荐
        int age2 = Integer.valueOf(ageStr);      // 也可用

        double score = Double.parseDouble("99.5");

        String rs1 = Integer.toString(23);       // int → String
        String rs2 = 23 + "";                    // 简便方式（底层也是 toString）
    }
}
```

```java
// 2. StringBuilder（高效字符串拼接）
public class Test1 {
    public static void main(String[] args) {
        StringBuilder s = new StringBuilder("itjk");

        s.append(12).append("zzz").append(true);  // 链式编程
        System.out.println(s);

        s.reverse();                               // 反转
        System.out.println(s.length());            // 长度

        String rs = s.toString();                  // 转 String
    }
}
```

```java
// 3. BigDecimal（解决浮点精度丢失）
import java.math.BigDecimal;
import java.math.RoundingMode;

public class Test2 {
    public static void main(String[] args) {
        double a = 0.1, b = 0.2;
        System.out.println(a + b);        // 0.30000000000000004 ❌

        // ✅ 推荐：BigDecimal.valueOf()
        BigDecimal a1 = BigDecimal.valueOf(a);
        BigDecimal b1 = BigDecimal.valueOf(b);

        BigDecimal c1 = a1.add(b1);       // 加法
        BigDecimal c2 = a1.subtract(b1);  // 减法
        BigDecimal c3 = a1.multiply(b1);  // 乘法
        BigDecimal c4 = a1.divide(b1);    // 除法（除不尽会抛异常）

        // 除法指定精度和舍入模式
        BigDecimal d1 = BigDecimal.valueOf(0.1);
        BigDecimal d2 = BigDecimal.valueOf(0.3);
        BigDecimal d3 = d1.divide(d2, 2, RoundingMode.HALF_UP);  // 0.33

        double db = d3.doubleValue();     // BigDecimal → double
    }
}
```

```java
// 4. Date 和 SimpleDateFormat
import java.util.Date;
import java.text.SimpleDateFormat;

public class Test2SimpleDateFormat {
    public static void main(String[] args) throws Exception {
        Date d = new Date();                      // 当前时间
        long time = d.getTime();                  // 毫秒值

        // 格式化：日期 → 字符串
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy年/MM月/dd日 HH时mm分ss秒");
        String rs = sdf.format(d);
        System.out.println(rs);

        // 解析：字符串 → 日期
        String dateStr = "2022-12-12 12:12:11";
        SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date d2 = sdf2.parse(dateStr);            // ❗ 格式必须一致
    }
}
```

```java
// 5. Arrays 工具类
import java.util.Arrays;

public class ArraysTest1 {
    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50, 60};

        // 数组内容转字符串
        System.out.println(Arrays.toString(arr));  // [10, 20, 30, 40, 50, 60]

        // 拷贝：指定范围（包前不包后）
        int[] arr2 = Arrays.copyOfRange(arr, 1, 4);
        System.out.println(Arrays.toString(arr2)); // [20, 30, 40]

        // 拷贝：指定新数组长度
        int[] arr3 = Arrays.copyOf(arr, 10);
        System.out.println(Arrays.toString(arr3)); // [10, 20, 30, 40, 50, 60, 0, 0, 0, 0]
    }
}
```

## 🧩 知识点拆解

### 知识点1：包装类

- **是什么**：8种基本类型对应的引用类型（`Integer`、`Double`、`Boolean` 等）
- **在代码中的作用**：用于泛型、字符串与基本类型互转
- **老师强调的要点**：
  - 自动装箱：`Integer a = 12`，自动拆箱：`int b = a`
  - `Integer` 缓存范围 -128~127，此范围内 `==` 比较为 true
  - `parseInt(String)` 和 `valueOf(String)` 的区别：前者返回 `int`，后者返回 `Integer`

### 知识点2：StringBuilder

- **是什么**：可变字符串，高效拼接，支持链式编程
- **在代码中的作用**：替代频繁 `+` 拼接 String 的性能问题
- **老师强调的要点**：
  - `StringBuilder` vs `StringBuffer`：前者非线程安全（更快），后者线程安全（更慢）
  - 连续 `append()` 返回自身对象 → 链式编程

### 知识点3：BigDecimal

- **是什么**：精确浮点数运算类
- **老师强调的要点**（**易错点！**）：
  - `double` 直接运算有精度丢失（`0.1+0.2≠0.3`）
  - 💡 **推荐**：`BigDecimal.valueOf(double)` 创建对象
  - ❌ **不推荐**：`new BigDecimal(double)`（仍有精度问题）
  - 除法除不尽时需指定精度和舍入模式

## ⚠️ 常见考题 / 易错点

1. **选择题——自动装箱拆箱**：`Integer a = null; int b = a;` → **NullPointerException**（拆箱时调用 `intValue()`，null 报错）

2. **选择题——StringBuilder vs StringBuffer**：
   - StringBuilder 非线程安全（更快）→ 单线程推荐
   - StringBuffer 线程安全（更慢）→ 多线程使用

3. **填空题——BigDecimal 创建方式**：推荐 `BigDecimal.valueOf(double)`，不推荐 `new BigDecimal(double)`

4. **编程题——日期格式化**：注意 `yyyy` 和 `YYYY` 的区别（Y 是"Week Year"，跨年周可能出错）

5. **选择题——parseInt 参数**：`Integer.parseInt("12")` 返回 **int**，不是 Integer

6. **编程题——StringBuilder 应用**：大量字符串拼接时用 StringBuilder 替代 String

---

#Java #编程笔记 #考点复习 #包装类 #StringBuilder #BigDecimal #日期 #Arrays
