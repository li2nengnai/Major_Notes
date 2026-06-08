---
created: 2026-06-08
source: E:\MY\IDE_wj\lianxi2\src\d1_file
tags: [java, file, io, 文件操作]
优先级: ⭐⭐⭐
---

# File 文件操作详解（小白友好版）

> 来源：自己 IDE 中的练习代码 `d1_file` 包
> 适合人群：Java 初学者

---

## 📖 本章内容

```toc
```

---

## 一、File 类是什么？

**一句话**：`File` 类是 Java 中用来**代表文件或文件夹路径**的工具。

> ⚠️ 注意：File 对象≠文件本身。它只是文件/文件夹的"路径代表"，就像快递单号代表一个包裹，但单号本身不是包裹。

**使用前先导包**：
```java
import java.io.File;
```

---

## 二、创建 File 对象（3种路径写法）

### 2.1 File 对象的基本创建

```java
File f = new File("文件/文件夹的路径");
```

### 2.2 三种路径写法对比

| 写法 | 示例 | 说明 | 推荐？ |
|------|------|------|:------:|
| **正斜杠** `/` | `"D:/resource/ab.txt"` | Java 中可以这样写，跨平台友好 | ✅ 推荐 |
| **双反斜杠** `\\` | `"D:\\resource\\ab.txt"` | Windows 风格，需要转义 | ✅ 也行 |
| **分隔符变量** | `"D:"+File.separator+"resource"` | 自动适配系统分隔符 | ✅ 最规范 |

```java
// 写法1：正斜杠（推荐，简单好记）
File f1 = new File("D:/resource/ab.txt");

// 写法2：双反斜杠（Windows 写法）
File f2 = new File("D:\\resource\\ab.txt");

// 写法3：用 File.separator（最规范，自动适配系统）
// Windows 是 \, Linux/Mac 是 /
File f3 = new File("D:" + File.separator + "resource" + File.separator + "ab.txt");
```

> 💡 `File.separator` 是系统自动的路径分隔符，Windows 是 `\`，Linux/Mac 是 `/`。

### 2.3 绝对路径 vs 相对路径

```java
// 🔴 绝对路径：从盘符开始的完整路径（带盘符）
File f4 = new File("D:\\code\\javasepromax\\file-io-app\\src\\itheima.txt");

// 🟢 相对路径（重点）：不带盘符，默认从"工程根目录"去找
// 假设你的工程名叫 myproject，那就从 myproject 文件夹开始找
File f5 = new File("file-io-app\\src\\abc.txt");
System.out.println(f5.getAbsoluteFile());  // 打印完整的绝对路径
```

> 🎯 **建议**：开发中用**相对路径**，部署时更灵活，换电脑不用改路径。

### 2.4 File 对象可以指代不存在的文件

```java
File f6 = new File("D:/resource/aaaa.txt");
System.out.println(f6.length());   // 0（文件不存在，长度为 0）
System.out.println(f6.exists());   // false（不存在）
```

> ⚠️ **File 对象不管文件存不存在都可以创建**，它只是代表一个路径。要判断文件是否存在，得用 `exists()` 方法。

---

## 三、File 类的常用方法（判断和获取信息）

### 3.1 快速概览

```java
File f1 = new File("D:/resource/ab.txt");
```

| 方法 | 作用 | 返回值 | 示例结果 |
|------|------|--------|---------|
| `exists()` | 文件/文件夹是否存在 | `boolean` | `true` |
| `isFile()` | 是否是**文件** | `boolean` | `true` |
| `isDirectory()` | 是否是**文件夹** | `boolean` | `false` |
| `getName()` | 获取文件/文件夹名 | `String` | `ab.txt` |
| `length()` | 获取文件大小（字节） | `long` | `3` |
| `lastModified()` | 最后修改时间（时间戳） | `long` | `1700000000000` |
| `getPath()` | 获取创建时用的路径 | `String` | `D:\resource\ab.txt` |
| `getAbsolutePath()` | 获取绝对路径 | `String` | `D:\resource\ab.txt` |

### 3.2 完整代码示例

```java
import java.io.File;
import java.text.SimpleDateFormat;

public class FileInfoDemo {
    public static void main(String[] args) {
        // 1. 创建 File 对象
        File f1 = new File("D:/resource/ab.txt");
        
        // 2. 判断是否存在
        System.out.println(f1.exists());          // true（存在）
        
        // 3. 判断是文件还是文件夹
        System.out.println(f1.isFile());          // true（是文件）
        System.out.println(f1.isDirectory());     // false（不是文件夹）
        
        // 4. 获取文件名
        System.out.println(f1.getName());         // "ab.txt"
        
        // 5. 获取文件大小（单位：字节）
        System.out.println(f1.length());          // 3（3个字节）
        
        // 6. 获取最后修改时间（返回的是时间戳，需要格式化）
        long time = f1.lastModified();
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
        System.out.println(sdf.format(time));     // 2024/01/15 10:30:00
        
        // 7. 获取路径
        File f2 = new File("D:\\resource\\ab.txt");
        System.out.println(f2.getPath());         // D:\resource\ab.txt（创建时用的路径）
        System.out.println(f2.getAbsolutePath()); // D:\resource\ab.txt（绝对路径）
        
        // 8. 相对路径的 File 对象获取信息
        File f3 = new File("file-io-app\\src\\itheima.txt");
        System.out.println(f3.getPath());         // file-io-app\src\itheima.txt（相对路径）
        System.out.println(f3.getAbsolutePath()); // D:\code\...\itheima.txt（完整的绝对路径）
    }
}
```

### 3.3 关于文件大小单位换算

```java
File f = new File("D:/resource/ab.txt");
long bytes = f.length();

// 单位换算
long kb = bytes / 1024;          // 1 KB = 1024 字节
long mb = kb / 1024;             // 1 MB = 1024 KB
long gb = mb / 1024;             // 1 GB = 1024 MB
```

---

## 四、File 类的常用方法（创建和删除）

### 4.1 快速概览

| 方法 | 作用 | 返回值 | 注意 |
|------|------|--------|------|
| `createNewFile()` | 创建新文件（空文件） | `boolean` | 文件已存在则返回 false |
| `mkdir()` | 创建**一级**文件夹 | `boolean` | 只能创建单级目录 |
| `mkdirs()` | 创建**多级**文件夹 | `boolean` | ✅ **推荐使用** |
| `delete()` | 删除文件或**空**文件夹 | `boolean` | ⚠️ 不能删非空文件夹 |

### 4.2 完整代码示例

```java
import java.io.File;

public class FileCreateDeleteDemo {
    public static void main(String[] args) throws Exception {
        
        // ========== 1. 创建文件 ==========
        File f1 = new File("D:/resource/itheima2.txt");
        System.out.println(f1.createNewFile());  
        // true（创建成功），如果文件已存在返回 false
        
        // ========== 2. 创建文件夹 ==========
        // mkdir()：只能创建一级文件夹
        File f2 = new File("D:/resource/aaa");
        System.out.println(f2.mkdir());  // true（创建成功）
        
        // mkdirs()：可以创建多级文件夹（推荐使用）
        File f3 = new File("D:/resource/bbb/ccc/ddd/eee/fff/ggg");
        System.out.println(f3.mkdirs()); // true（一次性创建所有层级）
        
        // ========== 3. 删除文件或空文件夹 ==========
        System.out.println(f1.delete());  // 删除文件：true
        System.out.println(f2.delete());  // 删除空文件夹：true
        
        // ⚠️ 删除非空文件夹 → 失败
        File f4 = new File("D:/resource");  // resource 里面有内容
        System.out.println(f4.delete());    // false（删不掉，因为不是空文件夹）
    }
}
```

### 4.3 `mkdir()` vs `mkdirs()` 区别（面试常问）

```java
File f1 = new File("D:/resource/aaa/bbb/ccc");

f1.mkdir();   // ❌ 失败！因为 mkdir 只能创建一级，而这里需要创建3级
f1.mkdirs();  // ✅ 成功！mkdirs 会创建所有不存在的父级目录
```

> **口诀**：`mkdir` 单级，`mkdirs` 多级。**日常开发无脑用 `mkdirs()`**。

---

## 五、File 类的常用方法（遍历文件夹）

### 5.1 快速概览

| 方法 | 作用 | 返回值 | 说明 |
|------|------|--------|------|
| `list()` | 获取所有**一级文件名** | `String[]` | 返回名字数组 |
| `listFiles()` | 获取所有**一级文件对象** | `File[]` | ✅ **重点**，返回 File 对象数组，可以继续操作 |

### 5.2 完整代码示例

```java
import java.io.File;

public class FileListDemo {
    public static void main(String[] args) {
        
        File f1 = new File("D:\\resource");
        
        // ========== 方法1：list() ==========
        // 返回 String 数组，只有文件名
        String[] names = f1.list();
        for (String name : names) {
            System.out.println(name);
        }
        // 输出类似：
        // ab.txt
        // itheima.txt
        // aaa
        
        System.out.println("=============================");
        
        // ========== 方法2：listFiles() ==========
        // 返回 File 数组，包含完整路径信息（重点掌握！）
        File[] files = f1.listFiles();
        for (File file : files) {
            System.out.println(file.getAbsolutePath());
            // 可以继续对 file 做各种操作
            // System.out.println(file.getName());
            // System.out.println(file.length());
        }
        // 输出类似：
        // D:\resource\ab.txt
        // D:\resource\itheima.txt
        // D:\resource\aaa
    }
}
```

### 5.3 两个方法的区别

| 对比 | `list()` | `listFiles()` ⭐ |
|------|----------|-----------------|
| 返回类型 | `String[]`（名字数组） | `File[]`（文件对象数组） |
| 能获取的信息 | 只有文件名 | 文件名+大小+路径+类型等 |
| 可以继续操作吗 | ❌ 不可以 | ✅ 可以（比如删除、改名） |
| **推荐吗** | ❌ 不推荐 | ✅ **强烈推荐** |

> 🎯 **listFiles() 才是重点**！因为返回的是 File 对象，拿到后可以继续调用 `getName()`、`length()`、`isFile()` 等方法。

---

## 六、完整总结

### File 类方法大全

```
创建 File 对象
├── new File(String pathname)        ← 根据路径创建
├── new File(String parent, String child)  ← 父路径+子路径

判断功能
├── exists()                         ← 是否存在
├── isFile()                         ← 是否是文件
├── isDirectory()                    ← 是否是文件夹

获取信息
├── getName()                        ← 文件名
├── length()                         ← 文件大小（字节）
├── lastModified()                   ← 最后修改时间
├── getPath()                        ← 获取构造路径
├── getAbsolutePath()                ← 获取绝对路径

创建/删除
├── createNewFile()                  ← 创建新文件
├── mkdir()                          ← 创建单级文件夹
├── mkdirs()                         ← 创建多级文件夹 ✅
├── delete()                         ← 删除文件/空文件夹

遍历
├── list()                           ← 获取文件名数组
├── listFiles()                      ← 获取文件对象数组 ⭐
```

### 使用场景总结

| 场景 | 用什么方法 |
|------|-----------|
| 想知道文件多大 | `length()` |
| 想知道是不是文件夹 | `isDirectory()` |
| 想创建一堆文件夹 | `mkdirs()` |
| 想遍历文件夹里的内容 | `listFiles()` |
| 想删除文件 | `delete()` |
| 想知道文件在哪儿 | `getAbsolutePath()` |

---

## ⚠️ 小白常见坑

### 坑1：File ≠ 文件
```java
File f = new File("D:/abc/def.txt");
// 此时硬盘上可能还没有这个文件！
System.out.println(f.exists());  // false
```

### 坑2：delete 不能删非空文件夹
```java
File f = new File("D:/resource");  // resource 里面有文件
f.delete();  // false！删不掉
// 想要删除非空文件夹，得先删里面的所有文件
```

### 坑3：路径分隔符用错
```java
// ❌ 错误：反斜杠不转义
File f = new File("D:\resource\ab.txt");  // 编译报错！

// ✅ 正确：用双反斜杠
File f = new File("D:\\resource\\ab.txt");

// ✅ 更推荐：用正斜杠
File f = new File("D:/resource/ab.txt");
```

---

## 📝 练习建议

1. 在你的电脑上找一个文件夹，用 `File` 对象打开它
2. 用 `listFiles()` 遍历里面的所有文件
3. 判断哪些是文件（`isFile()`），哪些是文件夹（`isDirectory()`）
4. 打印出每个文件的名字和大小
