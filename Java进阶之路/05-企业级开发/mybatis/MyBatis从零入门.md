---
created: 2026-06-09
source: E:\MY\IDE_wj\mybatis_v1
tags: [java, mybatis, orm, 数据库]
---

# MyBatis 从零入门（基于注解版）

## MyBatis 是什么？为什么要用它？

### 之前用 JDBC 写数据库有多痛苦

```java
// 纯 JDBC 查询一个用户 —— 全是样板代码
Connection conn = DriverManager.getConnection(url, user, password);
String sql = "SELECT * FROM user WHERE id = ?";
PreparedStatement ps = conn.prepareStatement(sql);
ps.setInt(1, 1);
ResultSet rs = ps.executeQuery();
User user = new User();
if (rs.next()) {
    user.setId(rs.getInt("id"));
    user.setUsername(rs.getString("username"));
    // ... 每个字段手动映射
}
// 还要关 rs, ps, conn
```

**痛点**：
- 代码大量重复（获取连接、创建语句、处理结果、关闭资源）
- SQL 和 Java 代码硬耦合在一起
- 结果集要**手动逐字段**赋值给对象

### MyBatis 做了什么

**一句话**：MyBatis 替你干了 JDBC 里所有重复劳动，你只需要写 SQL 和告诉它结果怎么映射。

```
你写的 → SQL 语句 + 参数
            ↓
MyBatis  → 帮你创建连接、执行SQL、封装结果
            ↓
你拿到的 → Java 对象（List<User>）
```

---

## 项目整体结构（先看全局）

```
mybatis_v1/
├── pom.xml                              ← Maven 依赖（引入 mybatis、mysql 驱动）
└── src/
    └── main/
        ├── java/com/demo/
        │   ├── entity/User.java         ← 实体类：对应数据库的 user 表
        │   ├── mapper/UserMapper.java   ← Mapper接口：写 SQL 的地方（核心！）
        │   ├── util/MyBatisUtil.java    ← 工具类：获取数据库连接
        │   └── App.java                 ← 启动类（本项目中未使用）
        └── resources/
            └── mybatis-config.xml       ← 全局配置文件：告诉 MyBatis 连哪个库
```

**MyBatis 的四块积木**（按顺序理解）：

```
① pom.xml（添加依赖）
    ↓
② mybatis-config.xml（连接数据库 + 配置）
    ↓
③ UserMapper.java（写 SQL 语句）
    ↓
④ 测试类（调用 Mapper 执行 SQL）
```

---

## 第一步：pom.xml —— 引入 MyBatis

```xml
<!-- MyBatis 核心 -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.13</version>
</dependency>

<!-- MySQL 驱动（MyBatis 用它连数据库） -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
</dependency>

<!-- 单元测试 -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
</dependency>
```

💡 **类比**：装软件前先装依赖，MyBatis 和 MySQL 驱动就是程序的"基础软件包"。

---

## 第二步：mybatis-config.xml —— 告诉 MyBatis 连哪个库

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 
        驼峰自动映射：数据库 create_time → Java createTime
        没这个的话，你得自己写 ResultMap 映射
    -->
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <!-- 控制台打印 SQL（调试神器） -->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <!-- 实体类别名：写代码时可以省略包名 -->
    <typeAliases>
        <package name="com.demo.entity"/>
    </typeAliases>

    <!-- 数据库连接信息 -->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/你的数据库名?..."/>
                <property name="username" value="root"/>
                <property name="password" value="你的密码"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 告诉 MyBatis 去哪里找 Mapper（SQL 所在的接口） -->
    <mappers>
        <package name="com.demo.mapper"/>
    </mappers>

</configuration>
```

### 配置里每个标签是干啥的

| 标签 | 作用 | 类比 |
|------|------|------|
| `<settings>` | 全局开关（驼峰/日志/缓存） | 手机设置 |
| `<typeAliases>` | 给实体类起短名 | 外号 |
| `<environments>` | 数据库连接信息 | 家庭住址 |
| `<mappers>` | 告诉 MyBatis 去哪里找 SQL | 目录索引 |

---

## 第三步：实体类 User —— 对应数据库表

```java
// 数据库 user 表 → Java 的 User 类 —— 一一对应
public class User {
    private Integer id;           // 对应数据库 id 字段
    private String username;      // 对应 username
    private Integer age;          // 对应 age
    private String email;         // 对应 email
    private Date createTime;      // 对应 create_time（驼峰自动映射！）

    // ⚠️ 必须有一个无参构造方法（MyBatis 反射创建对象时要用）
    public User() {}

    // 每个字段都要有 getter/setter（MyBatis 通过它们赋值）
    public Integer getId() { return id; }
    public void setId(Integer id) { this.id = id; }
    // ... 其他字段同理
}
```

### 为什么 User 类要这么写（三大要求）

| 要求 | 原因 |
|------|------|
| **字段名要匹配** | `username` → `username`，驼峰可自动转换 |
| **必须有无参构造** | MyBatis 用 `Class.newInstance()` 创建对象 |
| **必须有 getter/setter** | MyBatis 用 setter 注入查询结果，用 getter 获取参数值 |

---

## 第四步：Mapper 接口 —— 写 SQL 的地方（核心！）

这是整个 MyBatis **你最需要看懂的部分**：

```java
// Mapper = 映射器 —— 把 SQL 和 Java 方法绑定在一起
public interface UserMapper {

    // @Select 里面的 SQL，调用 selectAll() 时自动执行
    @Select("SELECT * FROM user")
    List<User> selectAll();

    // #{id} 是参数占位符，MyBatis 会自动替换成 ?
    // 相当于 PrepareStatement 的 setInt(1, id)
    @Select("SELECT * FROM user WHERE id = #{id}")
    User selectById(Integer id);

    // 新增：方法返回值 int 代表影响的行数
    @Insert("INSERT INTO user(username,age,email) VALUES(#{username},#{age},#{email})")
    int insert(User user);

    // 修改
    @Update("UPDATE user SET username=#{username},age=#{age},email=#{email} WHERE id=#{id}")
    int update(User user);

    // 删除
    @Delete("DELETE FROM user WHERE id=#{id}")
    int delete(Integer id);
}
```

### 这里到底发生了什么？（理解这个你就懂了）

```
你调用 mapper.selectById(1)
    ↓
MyBatis 看到 @Select("SELECT * FROM user WHERE id = #{id}")
    ↓
它把 #{id} 替换成你传的参数 1
    ↓
生成 SQL：SELECT * FROM user WHERE id = 1
    ↓
执行查询，拿到结果集
    ↓
看到返回类型是 User，自动把结果集的每一列 → User 的每个字段
    ↓
返回给你一个完整的 User 对象
```

### 四个注解的作用

| 注解 | SQL 类型 | 对应数据库操作 |
|------|---------|---------------|
| `@Select` | 查询 | 查（Read） |
| `@Insert` | 插入 | 增（Create） |
| `@Update` | 修改 | 改（Update） |
| `@Delete` | 删除 | 删（Delete） |

### `#{}` 占位符是什么意思

```java
@Select("SELECT * FROM user WHERE id = #{id}")
User selectById(Integer id);
//                  ↑ 这个参数的值会替换 #{id}

@Insert("INSERT INTO user(username) VALUES(#{username})")
int insert(User user);
// user.getUsername() 的值会替换 #{username}
```

**`#{}` = 占个坑，MyBatis 帮你填**。它自动用 `PreparedStatement` 的 `?` 替换，还能防 SQL 注入。

---

## 第五步：MyBatisUtil —— 工具类（固定模板）

这个类几乎每个 MyBatis 项目都一样，背下来就行：

```java
public class MyBatisUtil {
    // SqlSessionFactory 是重量级对象，整个应用只创建一次
    private static SqlSessionFactory sqlSessionFactory;

    // 静态代码块：项目启动时自动执行
    static {
        try {
            // 1. 读取 mybatis-config.xml 配置文件
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            // 2. 用配置文件构建 SqlSessionFactory
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // 获取 SqlSession（数据库会话）
    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession(false);  // false=手动提交事务
    }
}
```

### 三个核心对象的关系

```
mybatis-config.xml
       ↓ 读取
SqlSessionFactoryBuilder     ← 临时工，用完就扔
       ↓ build()
SqlSessionFactory            ← 永久工，整个项目只有一个
       ↓ openSession()
SqlSession                   ← 每次操作数据库创建一个，用完关
       ↓ getMapper()
UserMapper（代理对象）       ← 自动生成的"翻译官"
       ↓ 调用方法
SQL 执行，返回结果
```

---

## 第六步：测试类 —— 真刀真枪跑一遍

### 查询所有用户

```java
@Test
public void testSelectAll() {
    // 1. 获取 SqlSession（true = 自动提交事务）
    try (SqlSession session = MyBatisUtil.getSqlSession(true)) {
        // 2. 获取 Mapper（MyBatis 自动生成实现类）
        UserMapper mapper = session.getMapper(UserMapper.class);
        // 3. 调用方法 = 执行 SQL
        List<User> userList = mapper.selectAll();
        // 4. 遍历结果
        for (User u : userList) {
            System.out.println(u);
        }
    } // try-with-resources → 自动关闭 session
}
```

**三步曲（背下来！）**：

```
① session.getMapper(xxxMapper.class)   ← 拿到翻译官
② mapper.方法()                          ← 执行 SQL
③ 处理结果                               ← 拿数据干活
```

### 新增用户（注意事务提交）

```java
@Test
public void testInsert() {
    // 没传 true → 需要手动提交事务
    try (SqlSession session = MyBatisUtil.getSqlSession()) {
        UserMapper mapper = session.getMapper(UserMapper.class);
        
        User user = new User();
        user.setUsername("嘎嘎");
        user.setAge(31);
        user.setEmail("gaga@163.com");
        
        int rows = mapper.insert(user);  // 执行插入
        System.out.println("影响了" + rows + "行");
        
        session.commit();  // ⚠️ 必须提交事务，否则数据不写入数据库！
    }
}
```

### 为什么增删改要提交事务？

```
MyBatis 默认不自动提交
    ↓
insert/update/delete 执行后数据在"临时区"
    ↓
必须 session.commit() → 数据才真正写入数据库
    ↓
或者 openSession(true) → 自动提交
```

### 修改用户（先查再改）

```java
@Test
public void testUpdateById() {
    try (SqlSession session = MyBatisUtil.getSqlSession(true)) {
        UserMapper mapper = session.getMapper(UserMapper.class);
        
        // 1. 先查出用户
        User u = mapper.selectById(1);
        // 2. 修改字段
        u.setUsername("dongzi");
        u.setAge(18);
        u.setEmail("409@qq.com");
        // 3. 执行更新（根据 id 匹配）
        int num = mapper.update(u);
        System.out.println(num);
    }
}
```

---

## 完整流程总结（一张图）

```
┌─────────────────────────────────────────────────────┐
│                  你的 Java 代码                        │
│  UserMapper mapper = session.getMapper(...)           │
│  List<User> list = mapper.selectAll();                │
└────────────────────┬────────────────────────────────┘
                     ↓
┌─────────────────────────────────────────────────────┐
│                  MyBatis 替你干了                       │
│  1. 从连接池拿连接                                     │
│  2. 解析 @Select("SQL") → 获取 SQL 语句                │
│  3. 用 PreparedStatement 替换 #{} 占位符               │
│  4. 执行 SQL                                          │
│  5. 遍历 ResultSet，反射创建 User 对象，setter 赋值      │
│  6. 返回 List<User> 给你                               │
└─────────────────────────────────────────────────────┘
                     ↓
┌─────────────────────────────────────────────────────┐
│                   你拿到结果                            │
│  for (User u : list) { println(u); }                  │
└─────────────────────────────────────────────────────┘
```

---

## 常见问题（你没听懂的地方可能在这里）

### Q1：Mapper 是个接口，没有实现类，怎么调用的？

MyBatis 用 **JDK 动态代理** 自动生成了一个实现类。你调 `session.getMapper(UserMapper.class)` 时，MyBatis 在内存中创建了一个"代理对象"，它看到 `@Select("...")` 就知道要执行什么 SQL。**你不用写实现类，MyBatis 替你写了。**

### Q2：`#{}` 和 `${}` 有什么区别？

| 占位符 | 原理 | 安全性 |
|--------|------|--------|
| `#{id}` | 预编译 `?` | ✅ 防 SQL 注入 |
| `${id}` | 直接拼字符串 | ❌ 有注入风险 |

**永远用 `#{}`，别用 `${}`**。

### Q3：为什么实体类字段名和数据库不一样也能映射？

因为配置了：
```xml
<setting name="mapUnderscoreToCamelCase" value="true"/>
```
数据库 `create_time` → Java `createTime`，自动转驼峰。

### Q4：什么时候用 `session.commit()`？

| openSession() 参数 | 行为 |
|--------------------|------|
| `openSession(true)` | 自动提交，不用手动 commit |
| `openSession(false)` 或 `openSession()` | 手动提交，必须 `session.commit()` |

**查询不需要事务，增删改需要**。

---

## 你的代码里可以继续深入的点

1. **XML Mapper 方式**（替代注解）—— 复杂 SQL 用 XML 更灵活
2. **动态 SQL**（`<if>`、`<where>`、`<foreach>`）—— 条件查询必备
3. **多表联查**（`@Results` + `@Many`/`@One`）
4. **Spring Boot 整合 MyBatis**（实际开发都在 Spring Boot 里用）

---

#Java #MyBatis #ORM #数据库 #学习笔记
