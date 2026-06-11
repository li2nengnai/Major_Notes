---
title: File 文件操作详解
created: 2026-06-08
source: E:\MY\IDE_wj\lianxi2\src\d1_file
tags: [技术/Java/IO, 技术/Java/基础, 笔记/进阶之路]
---

# [[File]] 文件操作详解

## 核心概念

`File` 是文件/文件夹路径的代表，**≠ 文件本身**。

### 三种路径写法

```java
new File("D:/resource/ab.txt");               // 正斜杠（推荐）
new File("D:\\resource\\ab.txt");              // 双反斜杠（Windows）
new File("D:" + File.separator + "resource");  // 分隔符变量（跨平台）
```

### 绝对路径 vs 相对路径

- **绝对路径**：带盘符（如 `D:/a.txt`）
- **相对路径**：从项目根目录开始（**推荐**，换电脑不用改）

## 常用方法

### 判断/获取

| 方法 | 作用 |
|------|------|
| `exists()` | 是否存在 |
| `isFile()` | 是否是文件 |
| `isDirectory()` | 是否是文件夹 |
| `getName()` | 文件名 |
| `length()` | 文件大小（字节） |
| `lastModified()` | 最后修改时间 |

### 创建/删除

```java
f.createNewFile();    // 创建文件
f.mkdir();            // 创建单级目录
f.mkdirs();           // 创建多级目录（推荐）
f.delete();           // 删除文件或空文件夹
```

### 遍历

```java
String[] names = f.list();       // 文件名数组
File[] files = f.listFiles();    // File 对象数组（推荐）
```

## 踩坑记录

> [!warning] File 对象不代表文件真的存在
> ```java
> File f = new File("D:/abc.txt");
> System.out.println(f.exists());  // false（文件可能还没创建）
> ```

> [!warning] delete 不能删非空文件夹
> 想删非空文件夹，需要先递归删除里面的所有文件。

> [!warning] 路径分隔符用错
> ```java
> // ❌ \t 会被转义成制表符
> File f = new File("D:\test\ab.txt");
> // ✅
> File f = new File("D:/test/ab.txt");
> ```

## 知识依赖图

先学本篇 → 延伸 [[../../Java SE 考点复习/15-IO流|IO 流（读写文件内容）]]

---

#技术/Java/IO #技术/Java/基础 #笔记/进阶之路
