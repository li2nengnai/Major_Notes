---
created: 2026-06-08
topic: Java SE 考点复习 - 导航总页
tags: [Java, 编程笔记, 考点复习]
---

# Java SE 考点复习笔记 · 导航总页

> 笔记已按章节拆分为独立页面，点击下方链接进入各章节。

---

## 📖 章节导航

基础语法 → [[Java SE 考点复习/01-变量与数据类型|01 变量与数据类型]] · [[Java SE 考点复习/02-类型转换|02 类型转换]] · [[Java SE 考点复习/03-运算符|03 运算符]] · [[Java SE 考点复习/04-Scanner键盘录入|04 Scanner键盘录入]] · [[Java SE 考点复习/05-流程控制|05 流程控制]] · [[Java SE 考点复习/06-数组|06 数组]] · [[Java SE 考点复习/07-方法定义与重载|07 方法定义与重载]]

面向对象 → [[Java SE 考点复习/08-面向对象基础|08 OOP基础]] · [[Java SE 考点复习/09-String与ArrayList|09 String与ArrayList]] · [[Java SE 考点复习/10-static关键字|10 static关键字]] · [[Java SE 考点复习/11-继承多态抽象类接口|11 继承/多态/抽象类/接口]]

进阶API → [[Java SE 考点复习/12-常用API|12 常用API]] · [[Java SE 考点复习/13-异常处理|13 异常处理]] · [[Java SE 考点复习/14-集合框架|14 集合框架]] · [[Java SE 考点复习/15-IO流|15 IO流]] · [[Java SE 考点复习/16-网络编程与综合案例|16 网络编程+ATM]]

---

## 📌 课程模块

pk001-pk002 · 变量定义与数据类型 · **高频考点**

## 💻 核心代码示例

```java
// 1. 变量定义与使用
public class a004_VariableDemo1 {
    public static void main(String[] args) {
        // 数据类型 变量名 = 数据;
        // 注意：= 在Java中是赋值，从右往左执行
        int age = 23;                  // 定义一个整型变量，值为23
        System.out.println(age);

        double score = 99.5;           // 浮点型变量
        System.out.println(score);

        // 变量的特点：装的数据可以被替换
        int age2 = 18;
        age2 = 19;                     // 重新赋值（从右往左）
        age2 = age2 + 1;               // 先取age2值(19)+1=20，再赋给age2
        System.out.println(age2);      // 20

        // 应用场景：钱包金额变化
        double money = 9.5;
        money = money + 10;            // 收红包 → 19.5
        money = money - 5;             // 发红包 → 14.5
        System.out.println(money);
    }
}
```

```java
// 2. 变量注意事项
public class a005_VariableDemo2 {
    public static void main(String[] args) {
        // ① 变量必须先声明再使用
        int age = 21;
        age = 25;                      // 正确：重新赋值
        System.out.println(age);

        // ② 变量声明后，不能存其他类型数据
        // age = 35.9;                 // 编译报错！int不能存double

        // ③ 作用域：从定义开始到 } 截止，同一作用域不能重复定义同名变量
        {
            double money = 23.5;
            System.out.println(money);
            // double money = 10.4;    // 报错：重复定义
            // int age = 28;           // 报错：与外部age冲突
        }
        // System.out.println(money);  // 报错：超出作用域

        // ④ 定义时可不给初始值，但使用前必须赋值
        int number;
        number = 100;
        System.out.println(number);    // 正确：已赋值
    }
}
```

```java
// 3. 基本数据类型（ASCII编码 + 进制写法）
public class ASCIIDemo1 {
    public static void main(String[] args) {
        // ASCII编码：字符参与运算时使用对应的ASCII值
        System.out.println('a' + 10);  // 97 + 10 = 107
        System.out.println('A' + 10);  // 65 + 10 = 75
        System.out.println('0' + 10);  // 48 + 10 = 58

        // 进制在程序中的写法
        int a1 = 0B01100001;           // 二进制（0B开头）→ 97
        int a2 = 0141;                 // 八进制（0开头）→ 97
        int a3 = 0XFA;                 // 十六进制（0X开头）→ 250
        System.out.println(a1);
    }
}
```

## 🧩 知识点拆解

### 知识点1：变量声明与赋值

- **是什么**：变量是内存中存储数据的容器。格式：`数据类型 变量名 = 数据;`
- **在代码中的作用**：使用变量可以通过名称反复操作同一份数据，且值可变化
- **老师强调的要点**：
  - `=` 不是"等于"，是"赋值"，从右向左执行
  - `age2 = age2 + 1` 是先取右边 `age2` 的当前值，计算后再赋回去
  - 变量可以重复赋值，但不能改变数据类型

### 知识点2：变量的作用域

- **是什么**：变量从定义位置开始，到所在的花括号 `}` 结束，超出范围无法访问
- **在代码中的作用**：`money` 定义在内部 `{}` 中，外部 `{}` 无法访问
- **老师强调的要点**：
  - 内部代码块可以访问外部变量，但外部不能访问内部变量
  - 同一作用域内不能定义同名变量（哪怕是不同类型也不行）

### 知识点3：ASCII编码

- **是什么**：计算机存储字符时使用数字编码，`'a'=97`、`'A'=65`、`'0'=48`
- **在代码中的作用**：`'a' + 10` 实际运算是 `97 + 10 = 107`
- **老师强调的要点**：
  - 大小写字母相隔32：`'a' - 'A' = 32`
  - `'0'` 不等于 0，`'0'` 的 ASCII 是 48
  - 八进制写法容易混淆（0开头），建议避免使用

## ⚠️ 常见考题 / 易错点

1. **选择题/填空题——变量使用条件**：
   - 变量未声明直接使用 → **编译错误**
   - 变量声明后赋值类型不匹配 → **编译错误**（如 `int x = 3.14`）
   - 变量超出作用域使用 → **编译错误**

2. **选择题——ASCII值计算**：`'A' + 1` 的结果是？→ **66**（65+1）

3. **改错题——常见错误写法**：
   ```java
   int a; System.out.println(a);     // 错！使用前未赋值
   { int b = 5; } System.out.println(b); // 错！超出作用域
   ```

4. **编程题——考点**：变量交换（用临时变量）、变量累加（`sum += i`）

---

# 二、类型转换

## 📌 课程模块

pk002 · 自动类型转换与强制类型转换 · **高频考点** · **易错考点**

## 💻 核心代码示例

```java
// 1. 自动类型转换
public class TypeConversionDemo1 {
    public static void main(String[] args) {
        byte a = 12;                   // 1字节
        int b = a;                     // 自动转换：byte → int（小→大）
        System.out.println(b);         // 输出12

        int c = 100;                   // 4字节
        double d = c;                  // 自动转换：int → double（小→大）
        System.out.println(d);         // 100.0

        char ch = 'a';                 // 'a'的ASCII = 97
        int i = ch;                    // char → int 自动转换
        System.out.println(i);         // 97
    }
}
```

```java
// 2. 强制类型转换（有精度丢失风险）
public class TypeConversionDemo3 {
    public static void main(String[] args) {
        int a = 20;
        byte b = (byte) a;             // 强制类型转换：int → byte
        System.out.println(b);         // 20（数据未超范围，正常）

        int i = 1500;
        byte j = (byte) i;             // ❗ 数据溢出！byte范围 -128~127
        System.out.println(j);         // 输出异常值（高位截断）

        double d = 99.5;
        int m = (int) d;               // 强制转换：double → int
        System.out.println(m);         // 99（直接截断小数部分，不四舍五入）
    }
}
```

## 🧩 知识点拆解

### 知识点1：自动类型转换

- **是什么**：小范围类型自动变为大范围类型，无需手动干预
- **转换规则**：`byte → short → int → long → float → double`；`char → int`
- **在代码中的作用**：将 `byte` 赋值给 `int` 时自动扩展，数据无损
- **老师强调的要点**：
  - 自动转换的方向是**从小到大的方向**
  - `char` 可以自动转为 `int`，因为字符本质是数字（ASCII/Unicode）
  - 表达式中有多种类型时，结果自动升为最大类型

### 知识点2：强制类型转换

- **是什么**：大范围类型手动转为小范围类型，语法 `(目标类型) 数据`
- **在代码中的作用**：`(byte) i` 将 int 强制压缩为 byte
- **老师强调的要点**：
  - **可能丢失精度**：整数溢出（数据截断）、小数位丢失
  - **小数强转整数**：直接丢掉小数部分，**不是四舍五入**
  - 使用 IDEA 快捷键 `ALT + ENTER` 可快速添加强制转换

## ⚠️ 常见考题 / 易错点

1. **选择题——自动转换的方向**：以下哪个能自动转换？
   - `double → int` ❌（需强制）
   - `int → double` ✅
   - `byte → int` ✅
   - `long → float` ✅（float 范围比 long 大）

2. **选择题/填空题——强制转换结果**：
   ```java
   double d = 99.99;
   int i = (int) d;
   // i = ? → 99（截断小数，非四舍五入）
   ```

3. **改错题——溢出问题**：
   ```java
   byte b = 200;              // 编译错误！200超出byte范围(-128~127)
   byte b = (byte) 200;       // 编译通过，但运行时值异常
   ```

4. **出题方向**：算术运算中的类型提升
   ```java
   int a = 5;
   double b = 2;
   System.out.println(a / b); // 2.5（int + double → double）
   System.out.println(a / 2); // 2（int / int → int，结果取整）
   ```

---

# 三、运算符

## 📌 课程模块

pk002 · 算术运算符/逻辑运算符/短路特性 · **高频考点**

## 💻 核心代码示例

```java
// 1. 算术运算符 + 字符串连接符
public class OperatorDemo1 {
    public static void main(String[] args) {
        int a = 10, b = 2;
        System.out.println(a + b);       // 12
        System.out.println(a - b);       // 8
        System.out.println(a * b);       // 20
        System.out.println(a / b);       // 5（整数相除取整）
        System.out.println(5 / 2);       // 2 ❗ 不是2.5！
        System.out.println(5.0 / 2);     // 2.5（浮点数参与运算）
        System.out.println(1.0 * 5 / 2); // 2.5（乘1.0转为浮点）
        System.out.println(a % b);       // 0（取余/取模）

        // 字符串连接符 + （特别容易出错！）
        int a2 = 5;
        System.out.println("abc" + a2);      // "abc5"
        System.out.println(a2 + 5);          // 10（算数运算）
        System.out.println("abc" + a2 + 'a');// "abc5a"（从左到右）
        System.out.println(a2 + 'a' + "xx"); // 102xx ❓ 先算术后拼接
    }
}
```

```java
// 2. 逻辑运算符（短路特性是考点重点！）
public class OperatorDemo5 {
    public static void main(String[] args) {
        double size = 6.8;
        int storage = 16;

        boolean rs = size >= 6.95 & storage >= 8;  // & 两边都执行
        boolean rs2 = size >= 6.95 | storage >= 8;

        // 短路与 &&
        int i = 10, j = 20;
        System.out.println(i > 100 && ++j > 99);   // false
        // ❗ && 左边i>100为false，右边++j不再执行
        System.out.println(j);                       // 20（j没变）

        // 非短路 & 对比
        System.out.println(i > 100 & ++j > 99);     // false
        System.out.println(j);                       // 21（j执行了自增）

        // 短路或 ||
        int m = 10, n = 30;
        System.out.println(m > 3 || ++n > 40);      // true
        // ❗ || 左边为true，右边不再执行
        System.out.println(n);                       // 30（n没变）
    }
}
```

## 🧩 知识点拆解

### 知识点1：`+` 的两种身份

- **是什么**：`+` 既是算术加法运算符，又是字符串连接运算符
- **代码中的表现**：
  - 两边都是数值 → 算术加法 `5 + 5 = 10`
  - 任意一边是字符串 → 字符串连接 `"abc" + 5 = "abc5"`
- **老师强调的要点**（**高频考点！**）：
  - 运算从左到右依次执行
  - `5 + 'a' + "xx"` → 先算 `5 + 97 = 102` → 再 `102 + "xx" = "102xx"`
  - **关键判断**：字符串出现之前是算术运算，出现之后全是拼接

### 知识点2：短路运算符 `&&` 与 `||`

- **是什么**：`&&` 左边为 false 则右边不执行；`||` 左边为 true 则右边不执行
- **代码中的作用**：`i > 100 && ++j > 99` 中 `++j` 不会执行
- **老师强调的要点**（**必考题！**）：
  - `&&` 比 `&` 效率更高，常用 `&&` 和 `||`
  - 考题常结合 `++` 自增：判断右侧表达式是否执行
  - **短路特性**只会影响表达式的执行，不影响最终真假结果

## ⚠️ 常见考题 / 易错点

1. **选择题/填空题——字符串连接符陷阱**：
   ```java
   System.out.println("sum=" + 3 + 4);    // "sum=34"（拼接）
   System.out.println(3 + 4 + "=sum");    // "7=sum"（先算数）
   System.out.println("sum=" + (3 + 4));  // "sum=7"（先算括号）
   ```

2. **选择题——短路特性**：
   ```java
   int x = 5, y = 5;
   boolean b = (x > 10) && (y++ > 0);
   // y = ? → 5（y++没执行）
   ```

3. **改错题——整数除法**：
   ```java
   double avg = (80 + 90 + 70) / 3;     // avg = 80.0 ❌
   double avg = (80 + 90 + 70) / 3.0;   // avg = 80.0 ✅
   double avg = 1.0 * (80 + 90 + 70) / 3; // ✅
   ```

4. **编程题——取余数的应用**：判断奇偶 `num % 2 == 0`、判断倍数 `num % n == 0`

---

# 四、Scanner 键盘录入

## 📌 课程模块

pk002 · 键盘输入交互 · **理解考点**

## 💻 核心代码示例

```java
import java.util.Scanner;              // 1️⃣ 导包（IDEA自动导入）

public class ScannerDemo1 {
    public static void main(String[] args) {
        // 2️⃣ 创建键盘扫描器对象
        Scanner sc = new Scanner(System.in);

        // 3️⃣ 接收键盘输入
        System.out.println("请输入您的年龄：");
        int age = sc.nextInt();          // 等待输入整数，回车后获取

        System.out.println("请输入您的名字：");
        String name = sc.next();         // 等待输入字符串，回车后获取

        System.out.println(name + "欢迎您进入系统~");
    }
}
```

## 🧩 知识点拆解

### 知识点1：Scanner 三步法

- **是什么**：Java 提供 `java.util.Scanner` 类接收键盘输入
- **使用流程**：导包 → 创建对象 → 调用方法
- **老师强调的要点**：
  - `System.in` 代表键盘输入
  - `sc.nextInt()` 等待输入整数，`sc.next()` 等待输入字符串
  - 程序执行到 `nextInt()` 时会阻塞，等待用户按回车键

## ⚠️ 常见考题 / 易错点

1. **常见错误**：`nextInt()` 后使用 `nextLine()` 会读走换行符
2. **解决方案**：在 `nextInt()` 后加一个 `nextLine()` 吃掉换行
3. **出题方向**：结合分支循环做交互式菜单

---

# 五、流程控制（分支 + 循环）

## 📌 课程模块

pk003 · if/switch/for/while/do-while/break/continue · **高频考点**

## 💻 核心代码示例

```java
// 1. if 分支（三种形式）
public class IfDemo1 {
    public static void main(String[] args) {
        double t = 36.9;
        // 形式1：单if
        if(t > 37) {
            System.out.println("温度异常");
        }

        // 形式2：if-else
        double money = 19;
        if(money >= 90) {
            System.out.println("发红包成功");
        } else {
            System.out.println("余额不足");
        }

        // 形式3：if-else if-else（多条件判断）
        int score = 85;
        if(score >= 0 && score < 60) {
            System.out.println("D");
        } else if(score >= 60 && score < 80) {
            System.out.println("C");
        } else if(score >= 80 && score < 90) {
            System.out.println("B");
        } else if(score >= 90 && score <= 100) {
            System.out.println("A");
        } else {
            System.out.println("分数有误");
        }
    }
}
```

```java
// 2. switch 分支（支持String）
public class SwitchDemo2 {
    public static void main(String[] args) {
        String week = "周三";
        switch (week) {
            case "周一":
                System.out.println("埋头苦干");
                break;                     // ❗ 必须写break，否则穿透
            case "周二":
                System.out.println("找大牛帮忙");
                break;
            case "周三":
                System.out.println("今晚吃烧烤");
                break;
            // ... 其他case ...
            default:                       // 所有case不匹配时执行
                System.out.println("输入有误");
        }
    }
}
```

```java
// 3. for/while/do-while 三种循环对比
public class DoWhileDemo5 {
    public static void main(String[] args) {
        // for：先判断后执行（知道循环次数）
        for (int j = 0; j < 3; j++) {
            System.out.println("Hello");
        }
        // System.out.println(j); // ❌ 报错！j仅在for内有效

        // while：先判断后执行（不确定循环次数）
        int m = 0;
        while (m < 3) {
            System.out.println("Hello4");
            m++;
        }
        System.out.println(m);     // 3（m在外部定义，可访问）

        // do-while：先执行一次再判断（至少执行一次）
        int i = 0;
        do {
            System.out.println("Hello2");
            i++;
        } while (i < 3);
        // 特点：条件为false也会执行一次
        do {
            System.out.println("至少执行一次");
        } while (false);
    }
}
```

```java
// 4. break vs continue（必考对比！）
public class BreakAndContinueDemo8 {
    public static void main(String[] args) {
        // break：跳出并结束整个循环
        for (int i = 1; i <= 5; i++) {
            if(i == 3) break;             // i=3时直接结束循环
            System.out.println("我爱你：" + i);  // 只打印1,2
        }

        // continue：跳过本次，继续下次循环
        for (int i = 1; i <= 5; i++) {
            if(i == 3) continue;           // i=3时跳过打印，继续i=4
            System.out.println("洗碗：" + i);  // 打印1,2,4,5
        }
    }
}
```

```java
// 5. 循环嵌套（外层循环一次，内层循环一轮）
public class LoopNestedDemo7 {
    public static void main(String[] args) {
        // 外层3次 × 内层5次 = 共15次
        for (int i = 1; i <= 3; i++) {
            for (int j = 1; j <= 5; j++) {
                System.out.println("我爱你：" + i);
            }
            System.out.println("--------");
        }

        // 打印矩形
        for (int i = 1; i <= 4; i++) {       // 行
            for (int j = 1; j <= 40; j++) {  // 列
                System.out.print("*");
            }
            System.out.println();            // 换行
        }
    }
}
```

```java
// 6. Random 随机数
import java.util.Random;
public class RandomDemo1 {
    public static void main(String[] args) {
        Random r = new Random();

        int data = r.nextInt(10);             // [0, 9] 包前不包后
        System.out.println(data);

        // 生成 [1, 10]：先[0,9]再+1
        int data2 = r.nextInt(10) + 1;

        // 生成 [3, 17]：范围14个数(+15)，再偏移+3
        int data3 = r.nextInt(15) + 3;        // [0,14]+3 = [3,17]
    }
}
```

## 🧩 知识点拆解

### 知识点1：if vs switch 选择

- **是什么**：两者都是分支结构，`if` 处理范围判断，`switch` 处理等值匹配
- **老师强调的要点**：
  - `switch` 支持的数据类型：byte、short、int、char、String（JDK7+）、枚举
  - `switch` 的 `case` 穿透：没有 `break` 时会继续执行下一个 `case`
  - `if` 更适合区间条件（如 `score >= 60 && score < 80`）

### 知识点2：三种循环对比

- **是什么**：`for`（固定次数）、`while`（条件控制）、`do-while`（至少执行一次）
- **老师强调的要点**：
  - `for` 的循环变量在循环内定义，外部无法访问
  - `while` 循环变量在外部定义，外部可访问
  - `do-while` 即使条件为 false 也**至少执行一次**
  - **面试/考试**：三种循环可以互相替换，选择适合场景的即可

### 知识点3：break vs continue

- **是什么**：`break` 结束整个循环；`continue` 跳过本次循环的剩余语句
- **老师强调的要点**（**必考！**）：
  - `break` 跳出当前整个循环，不再执行后续任何次
  - `continue` 只是跳过"这一次"，继续下一次循环
  - `break` 和 `continue` 只能用在循环中（`break` 还可用在 `switch`）
  - 嵌套循环中，`break` 只跳出当前所在的循环层

### 知识点4：Random 公式

- **是什么**：`r.nextInt(n)` 生成 `[0, n)` 区间的随机整数
- **通用公式**：生成 `[min, max]` → `r.nextInt(max - min + 1) + min`
- **老师强调的要点**：
  - 包前不包后 `[0, n)`：`nextInt(10)` 只能取到 0~9
  - 生成随机用 Math 的 `Math.random()` 返回 `[0.0, 1.0)`

## ⚠️ 常见考题 / 易错点

1. **选择题——switch 穿透**：
   ```java
   int x = 1;
   switch(x) {
       case 1: System.out.print("A");
       case 2: System.out.print("B"); break;
       case 3: System.out.print("C");
   }
   // 输出：AB（case 1没有break，穿透到case 2）
   ```

2. **选择题——for 循环执行顺序**：
   ```java
   for(①初始化; ②条件; ④迭代) { ③循环体 }
   // 执行顺序：① → ②(→③→④→②) 循环 → ②不成立退出
   ```

3. **填空题——while 循环计数**：
   ```java
   int i = 0;
   while(i < 5) { i++; }
   // 循环结束时 i = 5
   ```

4. **改错题——死循环**：
   ```java
   for(;;) {}                 // 合法死循环（没有条件判断）
   while(true) {}             // 常用死循环写法
   ```

5. **编程题——外层×内层**：打印乘法口诀表、打印直角三角形（嵌套循环经典题）

6. **编程题——Random 猜数字**：生成随机数，循环猜数字直到猜中

---

# 六、数组

## 📌 课程模块

pk004 · 数组定义/内存原理/数组拷贝 · **高频考点**

## 💻 核心代码示例

```java
// 1. 数组定义与访问
public class ArrayDemo {
    public static void main(String[] args) {
        // 静态初始化
        int[] arr = {11, 22, 33};          // 完整写法：new int[]{11,22,33}
        System.out.println(arr);           // 打印地址值 [I@...
        System.out.println(arr[1]);        // 22

        arr[0] = 44;                       // 修改元素
        System.out.println(arr[0]);        // 44
    }
}
```

```java
// 2. 数组反转（经典题型）
public class Test2 {
    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50};
        // 双指针：i从前往后，j从后往前
        for (int i = 0, j = arr.length - 1; i < j; i++, j--) {
            // 交换 arr[i] 和 arr[j]
            int temp = arr[j];
            arr[j] = arr[i];
            arr[i] = temp;
        }

        // 遍历输出
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");  // 50 40 30 20 10
        }
    }
}
```

```java
// 3. 数组拷贝（深拷贝 vs 引用赋值）
public class Test5 {
    public static void main(String[] args) {
        int[] arr = {11, 22, 33};

        // ✅ 正确方式：创建新数组，逐元素拷贝
        int[] arr2 = copy(arr);
        arr2[1] = 666;
        System.out.println(arr[1]);          // 22（原数组不受影响）

        // ❌ 错误方式：只是复制引用（指向同一数组）
        // int[] arr3 = arr;
        // arr3[1] = 666;
        // System.out.println(arr[1]);        // 666（原数组被改了！）
    }

    public static int[] copy(int[] arr) {
        int[] newArr = new int[arr.length];  // 创建等长新数组
        for (int i = 0; i < arr.length; i++) {
            newArr[i] = arr[i];             // 逐个元素复制
        }
        return newArr;
    }
}
```

## 🧩 知识点拆解

### 知识点1：数组的两种初始化方式

- **是什么**：
  - 静态初始化：`int[] arr = {1, 2, 3};`（已知元素内容）
  - 动态初始化：`int[] arr = new int[5];`（只指定长度，系统给默认值 0）
- **老师强调的要点**：
  - 数组一旦创建，长度不可变
  - 索引从 0 开始，最大为 `arr.length - 1`
  - 访问越界索引 → `ArrayIndexOutOfBoundsException`

### 知识点2：引用赋值 vs 深拷贝

- **是什么**：
  - 引用赋值：`arr2 = arr` → 两个变量指向堆中同一数组，改一个全改
  - 深拷贝：创建新数组，逐个元素复制 → 修改互不影响
- **老师强调的要点**（**高频考点！**）：
  - 引用赋值不创建新数组，只是复制地址值
  - 数组拷贝的 **应用场景**：保护原始数据不被动

## ⚠️ 常见考题 / 易错点

1. **运行结果题——数组遍历**：以下输出什么？
   ```java
   int[] arr = new int[3];
   arr[0] = 1; arr[1] = 2;
   System.out.println(arr[2]);  // 0（动态初始化的默认值）
   ```

2. **改错题——数组越界**：`int[] arr = {1,2,3}; arr[3] = 4;` → 运行时异常

3. **选择题——引用赋值**：
   ```java
   int[] a = {1,2,3};
   int[] b = a;
   b[0] = 999;
   System.out.println(a[0]);  // 999（b和a指向同一数组）
   ```

4. **编程题——数组最大值**：遍历比较找最大

5. **编程题——评委打分**：总分减最高最低分后求平均（pk006/Test3.java）

---

# 七、方法定义与重载

## 📌 课程模块

pk005 · 方法定义/参数传递/返回值/重载 · **高频考点**

## 💻 核心代码示例

```java
// 1. 方法定义与调用
public class MethodDemo1 {
    public static void main(String[] args) {
        // 调用方法：接收返回值
        int rs = sum(10, 20);
        System.out.println("和是：" + rs);    // 30
    }

    // 定义方法：方法名小写开头，驼峰命名
    public static int sum(int a, int b) {
        int c = a + b;
        return c;                           // 返回结果给调用处
    }
}
```

```java
// 2. return 单独使用（结束无返回值方法）
public class ReturnDemo1 {
    public static void main(String[] args) {
        chu(10, 0);
    }

    public static void chu(int a, int b) {
        if (b == 0) {
            System.out.println("不能除0");
            return;                         // ❗ 跳出并结束方法，后面代码不执行
        }
        int c = a / b;
        System.out.println("结果是：" + c);
    }
}
```

```java
// 3. 参数传递机制：值传递（基本类型传递的是值的副本）
public class MethodDemo1 {
    public static void main(String[] args) {
        int a = 10;
        change(a);
        System.out.println("main:" + a);     // 10 ❗ 原值不变
    }

    public static void change(int a) {       // 这里的a是形参，是10的副本
        System.out.println("change1:" + a);  // 10
        a = 20;                              // 修改的是形参的值
        System.out.println("change2:" + a);  // 20
    }
}
```

```java
// 4. 方法重载 Overload（同名不同参）
public class MethodOverLoadDemo1 {
    // 以下方法构成重载：方法名相同，参数不同
    public static void test() { }
    void test(int a) { }                     // 参数个数不同
    void test(double a) { }                  // 参数类型不同
    void test(double a, int b) { }           // 参数个数+类型
    void test(int b, double a) { }           // 参数顺序不同
    int test(int a, int b) {                 // 返回值不同 ❗ 但仅有返回值不同不构成重载
        return a + b;
    }

    // ❌ 以下不构成重载（参数名不同不算）
    // int test(int c, int d) { }
    // 编译报错：方法签名重复
}
```

## 🧩 知识点拆解

### 知识点1：方法的完整定义格式

- **是什么**：`修饰符 返回值类型 方法名(参数列表) { 方法体; return 值; }`
- **老师强调的要点**：
  - 有返回值方法必须写 `return 值;`
  - 无返回值方法用 `void`，可写 `return;` 提前结束
  - 方法不调用不执行

### 知识点2：值传递机制

- **是什么**：Java 的参数传递**都是值传递**，传递的是值的副本
- **代码中的作用**：`change(a)` 把 `a` 的副本传给方法形参，方法内部修改不影响原变量
- **老师强调的要点**（**高频考点！**）：
  - 基本类型传递值副本 → 方法内修改不影响原变量
  - 引用类型（数组/对象）传递地址副本 → 方法内修改会影响原对象内容（地址相同）
  - 结合数组：传入数组，方法内修改数组元素 → **会影响原数组**

### 知识点3：方法重载

- **是什么**：同类中，方法名相同，参数列表不同（类型/个数/顺序），与**返回值无关**
- **老师强调的要点**（**必考题！**）：
  - 只有返回值不同**不构成**重载
  - 参数名不同**不构成**重载
  - 重载好处：用一个方法名表达同一类功能，调用更灵活

## ⚠️ 常见考题 / 易错点

1. **选择题——值传递**：
   ```java
   int x = 100;
   change(x);
   System.out.println(x);  // 100（基本类型值传递，原值不变）
   ```

2. **选择题——重载判断**：以下哪些构成重载？
   - `void test(int a)` 和 `void test(double a)` → ✅
   - `void test(int a)` 和 `int test(int a)` → ❌（仅返回值不同）
   - `void test(int a, int b)` 和 `void test(int b, int a)` → ❌（参数名不同不算）

3. **改错题——return 缺失**：
   ```java
   public static int sum(int a, int b) {
       // 缺少 return 语句 → 编译错误
   }
   ```

4. **编程题——方法设计**：注意返回值类型和参数类型匹配

---

# 八、面向对象（OOP）基础

## 📌 课程模块

pk007 · 类与对象/封装/构造器/this/JavaBean · **高频考点**

## 💻 核心代码示例

```java
// 1. 类定义：属性(成员变量) + 行为(成员方法)
public class Student {
    // 成员变量（对象的属性）
    String name;
    double chinese;
    double math;

    // 成员方法（对象的行为）
    public void printTotalScore() {
        System.out.println(name + "总成绩：" + (chinese + math));
    }

    public void printAverageScore() {
        System.out.println(name + "平均分：" + (chinese + math) / 2.0);
    }
}

// 测试类：创建对象并使用
public class Test {
    public static void main(String[] args) {
        // 1. 创建对象
        Student s1 = new Student();
        // 2. 给对象赋值
        s1.name = "播妞";
        s1.chinese = 100;
        s1.math = 100;
        // 3. 调用对象行为
        s1.printTotalScore();
        s1.printAverageScore();

        Student s2 = new Student();
        s2.name = "播仔";
        s2.chinese = 59;
        s2.math = 100;
        s2.printTotalScore();

        System.out.println(s1);  // 打印地址值
    }
}
```

```java
// 2. 封装：private + getter/setter + 构造器
public class Student {
    private String name;                  // ❗ private 私有化
    private double score;

    // 无参数构造器（如果没写，系统默认提供，但写了有参就必须手动写无参）
    public Student() { }

    // 有参数构造器
    public Student(String name, double score) {
        this.name = name;                 // this区分成员变量和参数
        this.score = score;
    }

    // getter/setter（右键 → Generate → Getter and Setter）
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getScore() {
        return score;
    }

    public void setScore(double score) {
        this.score = score;
    }
}
```

```java
// 3. this 关键字
public class Student {
    String name;
    double score;

    public void printPass(double score) {
        // this.score 是成员变量，score 是参数（局部变量）
        if (this.score >= score) {
            System.out.println("考入了大学");
        } else {
            System.out.println("没考过");
        }
    }
}
```

```java
// 4. 成员变量 vs 局部变量
public class Student {
    String name;           // 成员变量：类中方法外，有默认值(null)
    double score;          // 成员变量：有默认值(0.0)

    public void aaa() {
        double score = 98;                // 局部变量：方法内，必须赋值
        System.out.println(score);        // 98（就近原则：局部优先）
        System.out.println(this.score);   // 成员变量（用this区分）
    }
}
```

## 🧩 知识点拆解

### 知识点1：类与对象

- **是什么**：类是模板/蓝图，对象是根据类创建的具体实例
- **创建对象三步**：`类名 对象名 = new 类名();` → `对象名.成员变量` → `对象名.成员方法()`
- **老师强调的要点**：
  - `new` 是在堆内存中开辟空间
  - 一个类可以创建多个对象，各自独立
  - `s1.name` 和 `s2.name` 是不同的内存空间

### 知识点2：封装（private）

- **是什么**：用 `private` 隐藏成员变量，通过 `getter/setter` 暴露访问接口
- **在代码中的作用**：保护数据不被随意修改，可在 setter 中添加校验逻辑
- **老师强调的要点**：
  - `private` 只能在本类中访问
  - 提供 `public` 的 `getter/setter` 方法供外部访问
  - IDEA 快捷键：`ALT + INSERT` 快速生成

### 知识点3：构造器

- **是什么**：创建对象时自动调用的特殊方法，方法名与类名相同，无返回值
- **分类**：无参构造器（默认提供）、有参构造器
- **老师强调的要点**（**高频考点！**）：
  - 如果类中没有写任何构造器，系统默认提供无参构造器
  - **写了有参构造器后，系统不再提供无参构造器**，建议手动写上无参
  - 构造器可以重载

### 知识点4：this 关键字

- **是什么**：代表当前对象的引用
- **在代码中的作用**：`this.score` 访问成员变量，`score` 访问局部变量/参数
- **老师强调的要点**：
  - 谁调用方法，`this` 就指向谁
  - 主要用途：区分成员变量和方法参数

## ⚠️ 常见考题 / 易错点

1. **选择题——构造器定义**：以下哪个是合法的构造器？
   - `public void Student() {}` ❌（有返回值void，不是构造器）
   - `public Student() {}` ✅
   - `Student() {}` ✅（默认访问权限）

2. **选择题——默认值**：成员变量 int 的默认值是？→ **0**；String → **null**

3. **改错题——缺少无参构造器**：
   ```java
   public class Student {
       private String name;
       public Student(String name) { this.name = name; }
   }
   // Student s = new Student();  // ❌ 编译错误！写了有参构造后，无参被覆盖
   ```

4. **选择题——this 的作用**：以下关于 this 说法正确的是？
   - 用于区分成员变量和局部变量 ✅
   - 可以在静态方法中使用 ❌
   - 代表当前类的一个实例 ✅

5. **编程题——JavaBean 标准写法**：私有成员变量 + 无参构造 + getter/setter

---

# 九、String 与 ArrayList

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
  - 遍历——增删元素时注意索引变化

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

# 十、static 关键字

## 📌 课程模块

pk009 · 静态变量/静态方法/工具类/静态代码块/单例模式 · **高频考点**

## 💻 核心代码示例

```java
// 1. static 修饰成员变量：类变量 vs 实例变量
public class Student {
    static String name;                // 类变量：属于类，所有对象共享
    int age;                           // 实例变量：属于对象，每个对象独立
}

public class Test {
    public static void main(String[] args) {
        // 类变量：推荐类名访问
        Student.name = "袁华";

        Student s1 = new Student();
        s1.name = "马冬梅";              // 修改了类变量的值（所有对象共享）
        s1.age = 23;

        Student s2 = new Student();
        s2.name = "秋雅";                // 再次修改类变量

        System.out.println(s1.name);     // "秋雅"（类变量共享，改一个全改）
        System.out.println(Student.name);// "秋雅"（类名访问）
        System.out.println(s1.age);      // 23（实例变量独立）
        System.out.println(s2.age);      // 18（不同对象不同）
        // System.out.println(Student.age); // ❌ 报错！实例变量不能通过类名访问
    }
}
```

```java
// 2. 单例模式（饿汉式）
public class A {
    // 2. 类变量记住唯一实例
    private static A a = new A();

    // 1. 构造器私有
    private A() { }

    // 3. 公开静态方法获取实例
    public static A getObject() {
        return a;
    }
}

// 测试
class CC {
    void dd() {
        A a1 = A.getObject();
        A a2 = A.getObject();
        System.out.println(a1 == a2);     // true（同一个对象）
    }
}
```

## 🧩 知识点拆解

### 知识点1：类变量 vs 实例变量

- **是什么**：
  - 类变量（`static`）：属于类，所有对象共享一份数据，类加载时分配内存
  - 实例变量（无 `static`）：属于对象，每个对象独立一份，new 时分配内存
- **老师强调的要点**（**必考！**）：
  - 类变量推荐用 `类名.变量名` 访问（`Student.name`）
  - 类变量被所有对象共享，任意对象修改后，其他对象都受影响
  - 实例变量不能用类名访问（编译报错）

### 知识点2：单例模式

- **是什么**：确保一个类只有一个对象
- **三要素**：私有构造器 + 静态私有的实例变量 + 静态公开的获取方法
- **老师强调的要点**：
  - 饿汉式：类加载时就创建实例（线程安全）
  - 懒汉式：首次调用 `getObject()` 时才创建（需考虑线程安全问题）

## ⚠️ 常见考题 / 易错点

1. **选择题——static 变量共享**：
   ```java
   Student s1 = new Student();
   Student s2 = new Student();
   s1.count = 1; s2.count = 2;
   // 如果 count 是 static：s1.count = ? → 2
   // 如果 count 是非 static：s1.count = ? → 1
   ```

2. **填空题——静态方法限制**：静态方法中**不能直接访问**非静态成员，**不能使用 this**

3. **编程题——工具类设计**：构造器私有 + 所有方法静态（如 `Arrays`、`Collections`）

---

# 十一、继承、多态、抽象类、接口

## 📌 课程模块

pk009-pk010 · 继承/方法重写/多态/final/抽象类/接口 · **高频考点**

## 💻 核心代码示例

```java
// 1. 继承 extends
class B extends A { }          // B 继承 A，B 拥有 A 的非私有成员

// 2. 方法重写 @Override
public class Student {
    @Override
    public String toString() {           // 重写 Object 的 toString
        return "Student{name=" + name + "}";
    }
}
```

```java
// 3. 多态：对象多态 + 行为多态
public class Test {
    public static void main(String[] args) {
        // 对象多态：父类引用指向子类对象
        People p1 = new Teacher();
        People p2 = new Student();

        // ⭐ 行为多态：编译看左边（父类），运行看右边（子类方法）
        p1.run();                        // 调用的是 Teacher 的 run()
        p2.run();                        // 调用的是 Student 的 run()

        // ⭐ 变量：编译看左边，运行看左边（访问父类变量）
        System.out.println(p1.name);     // 访问的是 People 的 name
    }
}
```

```java
// 4. 抽象类 abstract + 模板方法设计模式
public abstract class People {
    // 模板方法（final修饰，子类不能重写）
    public final void write() {
        System.out.println("《我的爸爸》");
        System.out.println(writeMain()); // 调用抽象方法（子类实现）
        System.out.println("结尾");
    }

    // 抽象方法（没有方法体，子类必须实现）
    public abstract String writeMain();
}

public class Student extends People {
    @Override
    public String writeMain() {
        return "我的爸爸是超人...";
    }
}
```

```java
// 5. 接口 interface
public interface A {
    String SCHOOL_NAME = "极客程序员";    // 常量（public static final）
    void test();                          // 抽象方法（public abstract）
}

// 实现类
public class D implements A {
    @Override
    public void test() {
        System.out.println("实现方法");
    }
}
```

## 🧩 知识点拆解

### 知识点1：多态（重要！）

- **是什么**：同一个类型在不同时刻表现出不同形态
- **两个前提**：继承关系 + 方法重写
- **老师强调的要点**（**必考题！**）：
  - **行为看子类**：`p1.run()` 调用子类重写的方法
  - **变量看父类**：`p1.name` 访问父类的变量
  - 多态的优势：解耦，提高扩展性；劣势：不能调用子类特有方法

### 知识点2：抽象类 vs 接口

- **是什么**：两者都不能 new，都需要子类/实现类
- **老师强调的要点**：
  - 抽象类：`abstract class` → 可以有构造器、成员变量、具体方法
  - 接口：`interface` → 常量 + 抽象方法（JDK8+ 有 default/static 方法）
  - **选择**：有"is-a"关系用抽象类，有"like-a"能力用接口

## ⚠️ 常见考题 / 易错点

1. **选择题——多态调用结果**：
   ```java
   Animal a = new Dog();
   a.eat();    // 调用 Dog 的 eat（运行看右边）
   // a.watchDoor(); // ❌ 编译报错（编译看左边，父类没有此方法）
   ```

2. **选择题——重写 vs 重载**：
   - 重写（Override）：继承关系中，子类重写父类方法，方法签名相同
   - 重载（Overload）：同类中，方法名相同参数不同

3. **改错题——抽象类实例化**：`new A()` 其中 A 是抽象类 → **编译错误**

---

# 十二、常用 API（包装类 / 时间 / Math / BigDecimal / Arrays）

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
        String rs2 = 23 + "";                    // 简便方式
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

## ⚠️ 常见考题 / 易错点

1. **选择题——自动装箱拆箱**：`Integer a = null; int b = a;` → **NullPointerException**
2. **选择题——StringBuilder vs StringBuffer**：StringBuilder 非线程安全（更快），StringBuffer 线程安全（更慢）
3. **填空题——BigDecimal 创建方式**：推荐 `BigDecimal.valueOf(double)`，不推荐 `new BigDecimal(double)`
4. **编程题——日期格式化**：注意 `yyyy` 和 `YYYY` 的区别（Y 是"Week Year"）
5. **选择题——parseInt 参数**：`Integer.parseInt("12")` 返回 **int**

---

# 十三、异常处理

## 📌 课程模块

pk014 · 异常体系/分类/try-catch/throws/自定义异常 · **理解考点**

## 💻 核心代码示例

```java
// 1. throws 声明异常（编译时异常必须处理）
public class ExceptionTest1 {
    public static void main(String[] args) throws Exception {
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date d = sdf.parse("2028-11-11 10:24");   // ❗ 编译时异常，必须处理
    }
}
```

```java
// 2. 自定义异常
public class AgeIllegalException extends Exception {        // 编译时异常
    public AgeIllegalException(String message) {
        super(message);
    }
}

public class AgeIllegalRuntimeException extends RuntimeException {  // 运行时异常
    public AgeIllegalRuntimeException(String message) {
        super(message);
    }
}

// 使用异常
public class ExceptionTest2 {
    public static void main(String[] args) {
        try {
            saveAge2(225);
        } catch (AgeIllegalException e) {
            e.printStackTrace();       // 打印异常信息
        }
    }

    // throws 声明抛出
    public static void saveAge2(int age) throws AgeIllegalException {
        if (age <= 0 || age >= 150) {
            throw new AgeIllegalException("年龄不合法：" + age);
        }
        System.out.println("年龄保存成功：" + age);
    }

    // 运行时异常（无需throws声明）
    public static void saveAge(int age) {
        if (age <= 0 || age >= 150) {
            throw new AgeIllegalRuntimeException("年龄不合法：" + age);
        }
    }
}
```

```java
// 3. try-catch-finally（finally一定会执行）
public class Test1 {
    public static void main(String[] args) {
        try {
            System.out.println(10 / 2);
            // return;                   // finally 在 return 之前执行
            // System.exit(0);           // ❗ 只有这个能阻止 finally 执行
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            System.out.println("===finally执行了一次===");  // 无论是否异常都执行
        }
    }
}
```

## ⚠️ 常见考题 / 易错点

1. **选择题——finally 执行时机**：`try { return 1; } finally { return 2; }` → 返回 **2**（千万不要在 finally 中写 return）
2. **选择题——throws vs throw**：
   - `throws` 用在方法声明上，表示可能抛出异常
   - `throw` 用在方法体内，手动抛出异常对象
3. **选择题——运行时异常 vs 编译时异常**：RuntimeException 及其子类无需处理，其他必须处理

---

# 十四、集合框架（Collection / List / Set / Map）

## 📌 课程模块

pk015-pk016 · 集合体系/List/Set/Map/Collections · **高频考点**

## 💻 核心代码示例

```java
// 1. Collection 体系特点
public class CollectionTest1 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();  // 有序、可重复、有索引
        list.add("java1"); list.add("java2"); list.add("java1");
        System.out.println(list);                    // [java1, java2, java1]

        HashSet<String> set = new HashSet<>();       // 无序、不重复、无索引
        set.add("java1"); set.add("java2"); set.add("java1");
        System.out.println(set);                     // 随机顺序，只有1个java1
    }
}
```

```java
// 2. List 特有方法（索引操作）
public class ListTest1 {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("蜘蛛精");
        list.add("至尊宝");
        list.add("牛夫人");

        list.add(2, "紫霞仙子");            // 指定索引插入
        System.out.println(list.remove(2)); // 按索引删除并返回
        System.out.println(list.get(3));    // 获取指定索引元素
        System.out.println(list.set(3, "牛魔王")); // 修改并返回旧值
    }
}
```

```java
// 3. Set 三种实现
Set<Integer> set1 = new HashSet<>();        // 无序（哈希表）
Set<Integer> set2 = new LinkedHashSet<>();  // 有序（双向链表）
Set<Integer> set3 = new TreeSet<>();        // 可排序（红黑树）
```

```java
// 4. Map 集合
Map<String, Integer> map = new LinkedHashMap<>();
map.put("手表", 100);
map.put("手表", 220);                     // 键重复 → 值覆盖（220覆盖100）
map.put("手机", 2);
System.out.println(map.get("手表"));       // 220

Map<Integer, String> map2 = new TreeMap<>();  // 键可排序
map2.put(23, "Java"); map2.put(19, "李四");
```

```java
// 5. Collections 工具类
List<String> names = new ArrayList<>();
Collections.addAll(names, "张三", "王五", "李四");  // 批量添加
Collections.shuffle(names);                         // 打乱顺序
List<Integer> list = new ArrayList<>();
list.add(3); list.add(5); list.add(2);
Collections.sort(list);                             // 升序排序

// 自定义排序（Comparator）
Collections.sort(students, new Comparator<Student>() {
    @Override
    public int compare(Student o1, Student o2) {
        return Double.compare(o1.getHeight(), o2.getHeight());
    }
});
```

## ⚠️ 常见考题 / 易错点

1. **选择题——迭代器遍历时删除元素**：
   ```java
   // ❌ 错误：会抛 ConcurrentModificationException
   for (String s : list) { list.remove(s); }
   // ✅ 正确：使用 Iterator 的 remove()
   Iterator<String> it = list.iterator();
   while (it.hasNext()) { it.next(); it.remove(); }
   ```
2. **填空题——HashMap 底层**：数组 + 链表/红黑树（JDK8+）
3. **选择题——TreeSet 排序**：元素必须实现 `Comparable` 接口，或在构造时传入 `Comparator`

---

# 十五、IO 流（字节流 / 字符流 / 缓冲流 / 对象流）

## 📌 课程模块

pk018-pk019 · File/字节流/字符流/缓冲流/转换流/对象流 · **理解考点**

## 💻 核心代码示例

```java
// 1. File 对象
File f1 = new File("D:/resource/ab.txt");
System.out.println(f1.length());       // 文件大小（字节）
System.out.println(f1.exists());       // 是否存在

// 2. 字节流（通用所有文件）— 一次读取一个字节（性能差，汉字乱码）
InputStream is = new FileInputStream("file.txt");
int b;
while ((b = is.read()) != -1) {
    System.out.print((char) b);        // 汉字会乱码！一次读一个字节
}
is.close();

// 3. 字节流 — 一次读完所有字节（推荐小文件）
byte[] all = is.readAllBytes();
System.out.println(new String(all));

// 4. 缓冲流（提高性能，推荐方式）
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("src.txt"));
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("dest.txt"));
byte[] buffer = new byte[1024];
int len;
while ((len = bis.read(buffer)) != -1) {
    bos.write(buffer, 0, len);
}

// 5. 字符流（适合文本文件，无乱码）
try (Reader fr = new FileReader("test.txt")) {
    char[] buffer = new char[3];
    int len;
    while ((len = fr.read(buffer)) != -1) {
        System.out.print(new String(buffer, 0, len));
    }
}

// 6. 对象流 — 序列化
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("data.txt"));
oos.writeObject(user);                // 类必须实现 Serializable 接口

// 7. 反序列化
ObjectInputStream ois = new ObjectInputStream(new FileInputStream("data.txt"));
User u2 = (User) ois.readObject();
```

## ⚠️ 常见考题 / 易错点

1. **选择题——字节流 vs 字符流**：
   - 字节流：`FileInputStream` / `FileOutputStream` → 所有文件
   - 字符流：`FileReader` / `FileWriter` → 纯文本文件
2. **填空题——序列化要求**：类必须实现 `Serializable` 接口（标记接口，无抽象方法）
3. **编程题——资源关闭**：使用 try-with-resources 自动关闭，或在 finally 中手动 close
4. **选择题——缓冲流作用**：提高读写效率，减少磁盘 I/O 次数

---

# 十六、网络编程（Socket）

## 📌 课程模块

pk021 · Socket/ServerSocket/双向通信 · **理解考点**

## 💻 核心代码示例

```java
// 服务端
ServerSocket serverSocket = new ServerSocket(8888);
Socket clientSocket = serverSocket.accept();   // 等待客户端连接（阻塞）

// 客户端
Socket socket = new Socket("localhost", 8888); // 连接服务端

// 双向通信（多线程）
new Thread(new ReceiveHandler(socket)).start();  // 接收消息
new Thread(new SendHandler(socket)).start();     // 发送消息
```

## ⚠️ 常见考题 / 易错点

1. **填空题——Socket 编程流程**：服务端 `ServerSocket` → `accept()` 等待，客户端 `Socket` → 连接
2. **选择题——多线程通信**：收发分别用独立线程，避免阻塞

---

# 综合案例：ATM 系统

## 📌 课程模块

chapt002 · 综合项目实践 · **综合应用考点**

## 💻 核心代码示例

```java
// 卡号自动生成（8位不重复）
private String createCardId() {
    while (true) {
        String cardId = "";
        Random r = new Random();
        for (int i = 0; i < 8; i++) {
            cardId += r.nextInt(10);          // 数字拼接（先用着）
        }
        Account acc = getAccountByCardId(cardId);
        if (acc == null) return cardId;       // 无重复则返回
    }
}
// 改进建议：StringBuilder 替代字符串拼接
```

## ⚠️ 综合考题方向

1. **对象设计**：Account 类的封装（属性+getter/setter+构造器）
2. **集合应用**：ArrayList 存储多个账户
3. **流程控制**：菜单循环 + switch 分支 + 退出条件
4. **字符串比较**：密码/卡号比较用 `equals()`

---

#Java #编程笔记 #代码复盘 #考点复习 #考前冲刺
