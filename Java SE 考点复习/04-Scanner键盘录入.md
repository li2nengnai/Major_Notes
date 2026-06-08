---
created: 2026-06-08
topic: Java SE 考点复习 - Scanner 键盘录入
tags: [Java, 编程笔记, 考点复习, Scanner]
---

# Scanner 键盘录入

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
- **在代码中的作用**：实现人机交互，动态获取用户输入的数据
- **老师强调的要点**：
  - `System.in` 代表键盘输入
  - `sc.nextInt()` 等待输入整数，`sc.next()` 等待输入字符串
  - 程序执行到 `nextInt()` 时会阻塞，等待用户按回车键
  - IDEA 会自动导包，手动写用 `import java.util.Scanner;`

## ⚠️ 常见考题 / 易错点

1. **常见错误**：`nextInt()` 后使用 `nextLine()` 会读走换行符（`nextInt()` 只读取数字，留下换行符，`nextLine()` 直接读取到该换行符）
2. **解决方案**：在 `nextInt()` 后加一个 `nextLine()` 吃掉换行
3. **出题方向**：结合分支循环做交互式菜单（如 ATM 系统菜单）
4. **选择题——`next()` vs `nextLine()`**：
   - `next()`：读取到空格/回车停止（不能包含空格）
   - `nextLine()`：读取整行直到回车（可包含空格）

---

#Java #编程笔记 #考点复习 #Scanner #键盘输入
