---
created: 2026-06-08
topic: Java SE 基础阶段完整课堂笔记
source: 003_JavaSe_stage03_chapt001
---

# Java 2026-06-08 Java SE 基础阶段完整课堂笔记

## 一、核心知识点

### 1. Java 基础入门

- **注释**：单行 `//`、多行 `/* */`、文档注释 `/** */`
- **字面量**：整数、小数、字符（单引号且仅一个字符）、字符串（双引号）、布尔值（`true`/`false`）
- **特殊字符**：`\n` 换行、`\t` 制表符
- **变量**：`数据类型 变量名 = 数据;`，`=` 为赋值操作，从右向左执行
- **变量注意事项**：
  - 先声明再使用
  - 声明后不能存储其他类型的数据
  - 作用域从定义到 `}` 截止，同一作用域不能定义同名变量
  - 使用前必须有初始值

### 2. 数据类型与运算符

- **基本数据类型**：byte、short、int、long、float、double、char、boolean
- **ASCII 编码**：`'a'=97`、`'A'=65`、`'0'=48`，字符参与运算时使用 ASCII 值
- **进制表示**：二进制 `0B`、八进制 `0`、十六进制 `0X`
- **自动类型转换**：小范围 → 大范围自动转换（byte → int → double）
- **强制类型转换**：`(类型) 变量`，可能丢失精度
- **算术运算符**：`+ - * / %`，整数相除结果取整，`+` 可作为字符串连接符
- **逻辑运算符**：`&`(与)、`|`(或)、`!`(非)、`^`(异或)、`&&`(短路与)、`||`(短路或)
- **短路特性**：`&&` 左边为 false 右边不执行，`||` 左边为 true 右边不执行

### 3. 流程控制

- **if 分支**：`if` / `if-else` / `if-else if-else`
- **switch 分支**：支持 String 类型，每个 case 需 `break`，`default` 处理未匹配情况
- **for 循环**：`for(初始化; 条件; 迭代)`，先判断再执行
- **while 循环**：先判断条件再执行，适用于未知循环次数
- **do-while 循环**：先执行一次再判断条件
- **break**：跳出并结束当前所在循环
- **continue**：跳过当前当次循环，进入下一次
- **Random**：`r.nextInt(n)` 生成 `[0, n)` 区间的随机整数

### 4. 数组

- **定义**：`int[] arr = {11, 22, 33};` 或 `new int[]{11, 22, 33}`
- **访问**：通过索引 `arr[index]`，索引从 0 开始
- **内存原理**：数组引用存储在栈中，数组对象存储在堆中
- **常见操作**：遍历、拷贝（创建等长新数组并赋值）、求最大值/最小值
- **数组拷贝 vs 引用赋值**：直接 `arr2 = arr` 只是复制引用，指向同一数组

### 5. 方法

- **定义格式**：`修饰符 返回值类型 方法名(参数列表) { 方法体; return 值; }`
- **无返回值**：`void`，可使用 `return;` 结束方法
- **参数传递**：基本类型传递值，引用类型传递地址
- **方法重载**：同类中方法名相同，参数个数、类型、顺序不同，与返回值无关

### 6. 面向对象（OOP）

- **类与对象**：类 = 属性（成员变量）+ 行为（成员方法），对象根据类创建
- **封装**：用 `private` 隐藏细节，通过 `getter/setter` 访问
- **构造器**：无参构造器（默认提供）、有参构造器，用于对象初始化
- **this 关键字**：指向当前对象的引用，用于区分成员变量和局部变量
- **JavaBean**：私有成员变量 + getter/setter + 无参构造器

### 7. 常用 API

- **String**：不可变字符串，创建方式 `"直接量"` 或 `new String()`
  - 常用方法：`length()`、`charAt()`、`equals()`、`equalsIgnoreCase()`、`substring()`、`replace()`、`contains()`、`startsWith()`、`split()`
- **ArrayList**：`ArrayList<E> list = new ArrayList<>()`，泛型不支持基本类型
- **StringBuilder**：可变字符串，支持 `append()` 链式编程、`reverse()`、`toString()`

### 8. 包与导包

- **package**：声明类所在包，如 `package com.itjk.chapt001.pk001;`
- **import**：导入其他包中的类，`import java.util.Scanner;`

### 9. static 关键字

- **类变量**：`static` 修饰，属于类，通过 `类名.变量名` 访问
- **实例变量**：非 static，属于对象，需 new 后使用
- **静态方法**：只能访问静态成员，不能使用 `this`
- **工具类**：所有方法均为静态，构造器私有
- **静态代码块**：类加载时执行，仅一次
- **单例模式**：构造器私有 + 静态变量保存唯一实例 + 静态方法返回实例

### 10. 继承

- **extends**：子类继承父类，`class B extends A`
- **特点**：子类拥有父类的非私有成员；Java 单继承
- **方法重写（Override）**：子类重写父类方法，方法签名相同
- **super 关键字**：访问父类成员

### 11. 多态

- **对象多态**：`父类引用 = new 子类对象()`
- **行为多态**：编译看左边（父类），运行看右边（子类方法）
- **变量**：编译看左边，运行看左边（父类变量）

### 12. 抽象类与接口

- **抽象类**（`abstract`）：不能 new，可以有构造器、成员变量、抽象方法
- **抽象方法**：只有方法签名，无方法体，子类必须重写
- **模板方法模式**：抽象类定义模板方法（`final`），子类实现抽象方法（正文部分）
- **接口**（`interface`）：常量 + 抽象方法，不能 new，`implements` 实现
- **接口实现类**：必须重写所有抽象方法

### 13. 枚举

- **enum**：定义常量集合，`X, Y, Z;`，每个常量是枚举类的实例
- 可以有构造器、成员变量、方法

### 14. 常用工具类

- **Object**：`toString()`、`equals()`（比较内容需重写）
- **Integer 包装类**：自动装箱/拆箱，`parseInt()` 字符串转 int，`toString()` int 转字符串
- **Math**：`abs()`、`ceil()`、`floor()`、`round()`、`max()`、`min()`、`pow()`、`random()`
- **BigDecimal**：解决浮点数运算精度丢失问题
- **Date**：`new Date()` 获取当前时间，`getTime()` 获取毫秒值
- **SimpleDateFormat**：日期格式化，`yyyy-MM-dd HH:mm:ss`
- **Arrays**：数组工具类

### 15. 异常

- **异常分类**：编译时异常（受检异常）、运行时异常（`RuntimeException`）
- **处理方式**：`try-catch-finally`、`throws` 声明抛出
- **自定义异常**：继承 `Exception`（编译时）或 `RuntimeException`（运行时）
- **throw**：手动抛异常；**throws**：声明方法可能抛出的异常

### 16. 集合框架（Collection）

- **Collection 体系**：List（有序、可重复、有索引）、Set（无序/有序、不重复、无索引）
- **List 系列**：`ArrayList`、`LinkedList`，特有方法 `add(index)`、`remove(index)`、`get(index)`、`set(index, value)`
- **Set 系列**：`HashSet`（无序）、`LinkedHashSet`（有序）、`TreeSet`（可排序）
- **遍历方式**：迭代器（Iterator）、增强 for、forEach/Lambda

### 17. Map 集合

- **特点**：键值对存储，键不重复
- **实现类**：`HashMap`（无序）、`LinkedHashMap`（有序）、`TreeMap`（可排序）
- **常用方法**：`put()`、`get()`、`remove()`、`keySet()`、`entrySet()`
- **集合嵌套**：集合中嵌套集合（如 Map 中嵌套 List）

### 18. File 与 IO 流

- **File**：代表文件或目录，构造 `new File(路径)`，相对路径从工程根目录开始
- **递归**：方法自己调用自己，用于目录遍历、阶乘等
- **字节流**：`FileInputStream` / `FileOutputStream`，适合所有类型文件
- **字符流**：`FileReader` / `FileWriter`，适合纯文本文件
- **缓冲流**：`BufferedInputStream` / `BufferedOutputStream` / `BufferedReader` / `BufferedWriter`，提高性能
- **转换流**：`InputStreamReader` / `OutputStreamWriter`，指定编码
- **打印流**：`PrintStream` / `PrintWriter`
- **数据流**：`DataInputStream` / `DataOutputStream`，读写基本数据类型
- **对象流**：`ObjectInputStream` / `ObjectOutputStream`，序列化/反序列化对象
- **try-with-resources**：自动关闭资源，`try (Resource r = new Resource()) { }`

### 19. Properties 与日志

- **Properties**：读取/写入配置文件（`.properties`），`load(Reader)`、`getProperty(key)`、`stringPropertyNames()`
- **LogBack**：日志框架，`LoggerFactory.getLogger()`、`LOGGER.info()` / `debug()` / `error()`

### 20. 网络编程

- **Socket**：`ServerSocket` 服务端监听端口，`Socket` 客户端连接
- **双向通信**：多线程分别处理发送和接收
- **流程**：服务端 `accept()` 等待连接 → 获取 `InputStream`/`OutputStream` → 读写数据

### 21. ATM 系统综合案例

- 综合运用：ArrayList 存储账户、类封装（Account）、面向对象设计
- 功能：开户、登录、查询、存款、取款、转账、密码修改、销户
- 卡号自动生成（Random 生成 8 位不重复数字）

## 二、案例代码

### 2.1 HelloWorld

```java
public class a001_HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

### 2.2 注释与字面量

```java
public class a002_NoteDemo {
    public static void main(String[] args) {
        // 单行注释
        System.out.println("我开始学习Java程序了~~~");
        /*
            多行注释
        */
    }
}
```

```java
// 字面量演示
public class a003_LiteralDemo {
    public static void main(String[] args) {
        System.out.println(666);          // 整数
        System.out.println(3.14);         // 小数
        System.out.println('a');          // 字符
        System.out.println("黑马程序员"); // 字符串
        System.out.println(true);         // 布尔值
        System.out.println('\n');         // 换行符
        System.out.println('\t');         // 制表符
    }
}
```

### 2.3 变量与类型转换

```java
// 变量基本使用
int age = 23;
double score = 99.5;
age = 19;               // 变量可被替换
age = age + 1;          // 从右往左计算

// 自动类型转换
byte a = 12;
int b = a;              // byte → int 自动转换
int c = 100;
double d = c;           // int → double 自动转换

// 强制类型转换
int i = 1500;
byte j = (byte) i;      // 可能丢失精度
double d = 99.5;
int m = (int) d;        // 丢掉小数部分 → 99
```

### 2.4 Scanner 键盘输入

```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);
int age = sc.nextInt();       // 输入整数
String name = sc.next();      // 输入字符串
```

### 2.5 运算符

```java
// 算术运算符
int a = 10, b = 2;
System.out.println(a + b);    // 12
System.out.println(a / b);    // 5
System.out.println(5 / 2);    // 2（整数除法）
System.out.println(5.0 / 2);  // 2.5

// 字符串连接符
System.out.println("abc" + 5);       // "abc5"
System.out.println(5 + 'a' + "hei"); // "102hei"（先算术再拼接）

// 逻辑运算符（短路特性）
int i = 10;
System.out.println(i > 100 && ++j > 99);  // && 左边 false，右边不执行
System.out.println(i > 3 || ++n > 40);    // || 左边 true，右边不执行
```

### 2.6 if - switch 分支

```java
// if 分支
double t = 36.9;
if(t > 37) {
    System.out.println("温度异常");
} else {
    System.out.println("体温正常");
}

// switch 分支（支持 String）
String week = "周三";
switch (week) {
    case "周一":
        System.out.println("埋头苦干");
        break;
    case "周三":
        System.out.println("今晚吃烧烤");
        break;
    default:
        System.out.println("输入有误");
}
```

### 2.7 循环

```java
// for 循环
for(int i = 0; i < 3; i++) {
    System.out.println("Hello World");
}

// break: 跳出循环
for (int i = 1; i <= 5; i++) {
    if(i == 3) break;       // 到第3句结束
    System.out.println(i);
}

// continue: 跳过当次
for (int i = 1; i <= 5; i++) {
    if(i == 3) continue;    // 第3天不洗
    System.out.println("洗碗：" + i);
}
```

### 2.8 Random 随机数

```java
import java.util.Random;
Random r = new Random();
int data = r.nextInt(10);       // [0, 9]
int data2 = r.nextInt(10) + 1;  // [1, 10]
int data3 = r.nextInt(15) + 3;  // [3, 17]
```

### 2.9 数组

```java
// 定义数组
int[] arr = {11, 22, 33};
System.out.println(arr[0]);     // 11
arr[0] = 44;                    // 修改元素

// 数组拷贝
public static int[] copy(int[] arr) {
    int[] newArr = new int[arr.length];
    for (int i = 0; i < arr.length; i++) {
        newArr[i] = arr[i];
    }
    return newArr;
}
```

### 2.10 方法定义与重载

```java
// 方法定义
public static int sum(int a, int b) {
    return a + b;
}

// return 单独使用（无返回值方法中结束方法）
public static void chu(int a, int b) {
    if(b == 0) {
        System.out.println("不能除0");
        return;    // 结束方法
    }
    int c = a / b;
    System.out.println("结果是：" + c);
}

// 方法重载
void test() { }
void test(int a) { }
void test(double a, int b) { }
void test(int b, double a) { }
int test(int a, int b) { return a + b; }
```

### 2.11 面向对象 - 类定义与使用

```java
// 定义类
public class Student {
    String name;
    double chinese;
    double math;

    public void printTotalScore(){
        System.out.println(name + "总成绩：" + (chinese + math));
    }

    public void printAverageScore(){
        System.out.println("平均分：" + (chinese + math) / 2.0);
    }
}

// 创建对象使用
Student s1 = new Student();
s1.name = "播妞";
s1.chinese = 100;
s1.math = 100;
s1.printTotalScore();
```

### 2.12 封装与构造器

```java
// JavaBean 标准写法
public class Student {
    private String name;
    private double score;

    public Student() { }   // 无参构造

    public Student(String name, double score) {
        this.name = name;
        this.score = score;
    }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public double getScore() { return score; }
    public void setScore(double score) { this.score = score; }
}

// this 关键字
public void printPass(double score) {
    if(this.score >= score) {    // this.score 成员变量
        System.out.println("通过");
    }
}
```

### 2.13 String 常用方法

```java
String s = "Java";
System.out.println(s.length());              // 4
char c = s.charAt(1);                        // 'a'
char[] chars = s.toCharArray();

String s1 = new String("hello");
String s2 = new String("hello");
System.out.println(s1 == s2);                // false
System.out.println(s1.equals(s2));           // true
System.out.println("ABC".equalsIgnoreCase("abc")); // true

String sub = "Java是最好的语言".substring(0, 8);
String info = "垃圾电影".replace("垃圾", "**");
System.out.println("Java".contains("Java"));
System.out.println("张三丰".startsWith("张"));

String[] arr = "张无忌,周芷若,赵敏".split(",");
```

### 2.14 ArrayList

```java
import java.util.ArrayList;

ArrayList<String> list = new ArrayList<>();
list.add("Java");
list.add(1, "MySQL");           // 指定位置插入
String rs = list.get(1);        // 获取
System.out.println(list.size());// 大小
list.remove(1);                 // 按索引删除
list.remove("Java");            // 按元素删除（仅删除第一个匹配）
list.set(1, "newValue");        // 修改
```

### 2.15 static 关键字

```java
public class Student {
    static String schoolName;    // 类变量，属于类
    int age;                     // 实例变量，属于对象
}

// 单例模式（饿汉式）
public class A {
    private static A a = new A();
    private A() { }
    public static A getObject() { return a; }
}
```

### 2.16 继承与方法重写

```java
// 继承
class B extends A {
    // 子类拥有父类非私有成员
}

// 方法重写（Override）：子类重写父类方法
public class Student {
    @Override
    public String toString() {
        return "Student{name=" + name + "}";
    }
}
```

### 2.17 多态

```java
People p1 = new Teacher();
p1.run();                  // 编译看左边，运行看右边（调用子类方法）
System.out.println(p1.name); // 变量：编译看左边，运行看左边（父类变量）
```

### 2.18 抽象类与模板方法

```java
public abstract class People {
    // 模板方法
    public final void write() {
        System.out.println("《我的爸爸》");
        System.out.println(writeMain());  // 子类实现具体内容
        System.out.println("结尾");
    }
    public abstract String writeMain();   // 抽象方法
}

public class Student extends People {
    @Override
    public String writeMain() {
        return "我的爸爸是超级英雄...";
    }
}
```

### 2.19 接口

```java
public interface A {
    String SCHOOL_NAME = "极客程序员";  // 常量
    void test();                        // 抽象方法
}

// 实现类
public class D implements A {
    @Override
    public void test() {
        System.out.println("实现接口方法");
    }
}
```

### 2.20 枚举

```java
public enum A {
    X, Y, Z;
    // 每个常量都是枚举类的一个实例
}
```

### 2.21 包装类

```java
Integer a = 12;                    // 自动装箱
int b = a;                         // 自动拆箱

// 字符串 ↔ 基本类型
String ageStr = "29";
int age = Integer.parseInt(ageStr);           // String → int
int age2 = Integer.valueOf(ageStr);
double score = Double.parseDouble("99.5");

String rs = Integer.toString(23);             // int → String
String rs2 = 23 + "";
```

### 2.22 StringBuilder

```java
StringBuilder s = new StringBuilder("itjk");
s.append(12).append("zzz").append(true);    // 链式编程
s.reverse();                                  // 反转
String rs = s.toString();                     // 转 String
```

### 2.23 Math 与 BigDecimal

```java
Math.abs(-12);       // 12
Math.ceil(4.1);      // 5.0 向上取整
Math.floor(4.9);     // 4.0 向下取整
Math.round(3.5);     // 4 四舍五入
Math.pow(2, 3);      // 8.0 次方
Math.random();       // [0.0, 1.0) 随机数

System.out.println(0.1 + 0.2);  // 0.30000000000000004 精度问题
```

### 2.24 Date 与 SimpleDateFormat

```java
import java.util.Date;
import java.text.SimpleDateFormat;

Date d = new Date();             // 当前时间
long time = d.getTime();         // 毫秒值

SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
Date d2 = sdf.parse("2028-11-11 10:24");
```

### 2.25 异常处理

```java
// try-catch 处理
try {
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    Date d = sdf.parse("2028-11-11 10:24");
} catch (ParseException e) {
    e.printStackTrace();
}

// throws 声明抛出
public static void saveAge2(int age) throws AgeIllegalException {
    if(age <= 0 || age >= 150) {
        throw new AgeIllegalException("年龄不合法");
    }
}

// 自定义异常
public class AgeIllegalException extends Exception {
    public AgeIllegalException(String message) {
        super(message);
    }
}
```

### 2.26 Collection 集合

```java
// List：有序、可重复、有索引
List<String> list = new ArrayList<>();
list.add("蜘蛛精");
list.add("至尊宝");
list.add(2, "紫霞仙子");       // 指定索引插入
System.out.println(list.get(3));
System.out.println(list.set(3, "牛魔王"));

// Set：不重复、无索引
Set<Integer> set = new HashSet<>();       // 无序
Set<Integer> set2 = new LinkedHashSet<>(); // 有序
Set<Integer> set3 = new TreeSet<>();       // 可排序（升序）

// 迭代器遍历
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String ele = it.next();
    System.out.println(ele);
}
```

### 2.27 Map 集合

```java
Map<String, Integer> map = new LinkedHashMap<>();
map.put("手表", 100);
map.put("手表", 220);        // 键重复 → 覆盖
map.put("手机", 2);
System.out.println(map.get("手表"));  // 220

Map<Integer, String> map2 = new TreeMap<>();  // 键可排序
map2.put(23, "Java");
map2.put(19, "李四");
```

### 2.28 File 与字节流

```java
import java.io.*;

// File 对象
File f1 = new File("D:/resource/ab.txt");
System.out.println(f1.length());
System.out.println(f1.exists());

// 字节输入流
InputStream is = new FileInputStream("file.txt");
int b;
while ((b = is.read()) != -1) {
    System.out.print((char) b);
}
is.close();

// 字节缓冲流
InputStream bis = new BufferedInputStream(new FileInputStream("src.txt"));
OutputStream bos = new BufferedOutputStream(new FileOutputStream("dest.txt"));
byte[] buffer = new byte[1024];
int len;
while ((len = bis.read(buffer)) != -1) {
    bos.write(buffer, 0, len);
}
```

### 2.29 字符流

```java
// 字符输入流（适合文本文件）
try (Reader fr = new FileReader("io-app2\\src\\itheima01.txt")) {
    char[] buffer = new char[3];
    int len;
    while ((len = fr.read(buffer)) != -1) {
        System.out.print(new String(buffer, 0, len));
    }
} catch (Exception e) {
    e.printStackTrace();
}
```

### 2.30 对象序列化

```java
// 序列化
ObjectOutputStream oos =
    new ObjectOutputStream(new FileOutputStream("data.txt"));
User u = new User("admin", "张三", 32, "666888xyz");
oos.writeObject(u);

// 反序列化
ObjectInputStream ois =
    new ObjectInputStream(new FileInputStream("data.txt"));
User u2 = (User) ois.readObject();
```

### 2.31 Properties 配置文件

```java
Properties properties = new Properties();
properties.load(new FileReader("users.properties"));

// 读取属性
System.out.println(properties.getProperty("赵敏"));

// 遍历
Set<String> keys = properties.stringPropertyNames();
for (String key : keys) {
    System.out.println(key + "=" + properties.getProperty(key));
}
```

### 2.32 LogBack 日志

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class LogBackTest {
    public static final Logger LOGGER = LoggerFactory.getLogger("LogBackTest");

    public static void main(String[] args) {
        try {
            LOGGER.info("方法开始执行~~~");
            chu(10, 0);
            LOGGER.info("方法执行成功~~~");
        } catch (Exception e) {
            LOGGER.error("方法执行失败~~~");
        }
    }

    public static void chu(int a, int b) {
        LOGGER.debug("参数a:" + a);
        LOGGER.debug("参数b:" + b);
        int c = a / b;
        LOGGER.info("结果是：" + c);
    }
}
```

### 2.33 Socket 网络通信

```java
// 服务端
try (ServerSocket serverSocket = new ServerSocket(8888)) {
    Socket clientSocket = serverSocket.accept();  // 等待连接
    // 多线程处理收发
    new Thread(new ReceiveHandler(clientSocket)).start();
    new Thread(new SendHandler(clientSocket)).start();
}

// 客户端
try (Socket socket = new Socket("localhost", 8888)) {
    new Thread(new ReceiveHandler(socket)).start();
    new Thread(new SendHandler(socket)).start();
}
```

### 2.34 ATM 系统（综合案例）

```java
// 功能：开户、登录、查询、存款、取款、转账、密码修改、销户
// 卡号自动生成：
private String createCardId() {
    while (true) {
        String cardId = "";
        Random r = new Random();
        for (int i = 1; i <= 8; i++) {
            cardId += r.nextInt(10);
        }
        Account acc = getAccountByCardId(cardId);
        if (acc == null) return cardId;   // 不重复则返回
    }
}
```

## 三、重点说明 & 常见误区

### 常见误区

1. **`==` 与 `equals()` 混淆**：`==` 比较引用地址，`equals()` 比较内容。String 比较内容务必用 `equals()`
2. **整数除法结果**：`5 / 2 = 2`（不是 2.5），需要小数结果需将操作数转为浮点：`5.0 / 2` 或 `1.0 * 5 / 2`
3. **`+` 连接符陷阱**：`"abc" + 5 + 'a'` → `"abc5a"`，但 `5 + 'a' + "abc"` → `"102abc"`（先算术运算）
4. **变量作用域**：在 `{}` 内定义的变量不能在 `}` 外使用；同一作用域不能定义同名变量
5. **switch 漏写 break**：导致 case 穿透执行后续所有 case
6. **数组越界**：索引从 0 开始，最大 `arr.length - 1`
7. **String 拼接性能**：大量字符串拼接使用 `StringBuilder`，不要直接用 `+` 拼接
8. **集合遍历时删除元素**：使用迭代器的 `remove()` 方法，不要用集合自身的 `remove()`（会触发 `ConcurrentModificationException`）
9. **强制类型转换丢失精度**：大范围→小范围需强制转换，但可能数据溢出或小数丢失
10. **float/double 精度问题**：`0.1 + 0.2 != 0.3`，金额计算使用 `BigDecimal`
11. **静态方法不能访问非静态成员**：static 方法中不能直接使用非 static 变量和方法
12. **异常捕获顺序**：多个 catch 时，子类异常在前，父类在后
13. **资源关闭**：IO 流使用完后必须关闭释放资源，推荐 try-with-resources
14. **序列化需实现 Serializable**：对象流只能操作实现了 Serializable 接口的类

### 考点提示

- **方法重载 vs 方法重写**：重载是同名不同参（同类），重写是子类重写父类方法（继承关系）
- **抽象类 vs 接口**：抽象类可包含实现方法；接口 JDK8 后可有 default/static 方法
- **List vs Set**：List 有序可重复有索引，Set 不重复无索引
- **HashMap vs TreeMap**：HashMap 基于哈希表（O(1)），TreeMap 基于红黑树（有序）
- **ArrayList vs LinkedList**：ArrayList 数组结构（查询快增删慢），LinkedList 链表结构（增删快查询慢）
- **字节流 vs 字符流**：字节流处理所有文件，字符流处理纯文本文件（解决乱码问题）

## 四、拓展补充

### 4.1 字符串池机制

```java
String s1 = "Java";                    // 字符串常量池
String s2 = "Java";
System.out.println(s1 == s2);          // true（常量池同一对象）

String s3 = new String("Java");
String s4 = new String("Java");
System.out.println(s3 == s4);          // false（堆中不同对象）
System.out.println(s3.equals(s4));     // true（内容相同）
```

### 4.2 try-with-resources 自动资源管理

JDK 7 引入的自动关闭资源语法，无需手动 `close()`：

```java
try (FileReader fr = new FileReader("test.txt");
     BufferedReader br = new BufferedReader(fr)) {
    // 使用资源
} catch (IOException e) {
    e.printStackTrace();
}   // 资源自动关闭
```

### 4.3 HashMap 底层原理

- 基于 **哈希表**（数组 + 链表/红黑树）
- `put()`：计算 key 的 hashCode → 确定桶位置 → 若冲突则链表/红黑树存储
- `get()`：计算 hashCode → 定位桶 → 遍历链表/红黑树通过 equals() 匹配
- 自定义类作为 key 时需要重写 `hashCode()` 和 `equals()`

### 4.4 Commons-IO 工具库

Apache Commons IO 简化了 IO 操作，提供 `FileUtils`、`IOUtils` 等工具类。

### 4.5 学习路径建议

```
基础语法 → 面向对象 → 常用API → 集合框架 → IO流 → 多线程/网络编程 → 综合项目
```

---

#Java #编程笔记 #代码复盘
