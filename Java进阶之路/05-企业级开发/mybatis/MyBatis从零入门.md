---
title: MyBatis 从零入门（注解+XML双模式）
created: 2026-06-09
source: E:\MY\IDE_wj\mybatis_v1 + 老师教程
tags: [技术/框架/MyBatis, 技术/Java/数据库, 技术/数据库/MySQL, 笔记/进阶之路]
---

# [[MyBatis]] 从零入门 —— 搭出第一个可运行项目

## 核心概念

### 这玩意儿是干啥的

[[MyBatis]] 是 Java 操作数据库的「工具人」。你负责写 [[SQL]]，它负责跑腿——替你连数据库、执行 SQL、把结果转成 [[Java]] 对象。

### 没有它之前有多痛苦

用纯 [[JDBC]] 查一条记录：

```java
// 真正有用的代码不到 30%，剩下全是样板
Connection conn = DriverManager.getConnection(URL, USER, PASSWORD);
PreparedStatement ps = conn.prepareStatement("SELECT * FROM user WHERE id = ?");
ps.setInt(1, 1);
ResultSet rs = ps.executeQuery();
User user = new User();
if (rs.next()) {
    user.setId(rs.getInt("id"));           // 手动映射每一列
    user.setUsername(rs.getString("username"));
    user.setEmail(rs.getString("email"));
}
rs.close();  // 手动关闭
ps.close();
conn.close();
```

**三个烦：** 代码重复烦、手动映射烦、关闭资源烦。[[MyBatis]] 把这三个烦全包了。

### 它怎么工作的

```
你的 Java 代码 → 调 Mapper 接口方法
                     ↓
MyBatis 自动：拿连接 → 解析 SQL → 替换参数 → 执行 → 封装结果
                     ↓
你直接拿到 List<User>，啥也不用管
```

---

## 语法格式 / 使用方式

### 第一步：建数据库和表

```sql
-- 复制这段 SQL 去 MySQL 里跑一遍，数据库就有了
CREATE DATABASE IF NOT EXISTS mybatis_demo DEFAULT CHARACTER SET utf8mb4;
USE mybatis_demo;

CREATE TABLE `user` (
    `id` INT PRIMARY KEY AUTO_INCREMENT,
    `username` VARCHAR(50) NOT NULL COMMENT '用户名',
    `age` INT COMMENT '年龄',
    `email` VARCHAR(50) COMMENT '邮箱',
    `create_time` DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'
) COMMENT '用户表';

INSERT INTO `user` (username, age, email) VALUES
('张三', 22, 'zhangsan@163.com'),
('李四', 25, 'lisi@163.com');
```

---

### 第二步：Maven 依赖（[[pom.xml]]）

> ⚠️ **版本说明**：以下依赖适用于 MySQL 8.0.x + [[MyBatis]] 3.5.x。如果你用 MySQL 5.x，驱动用 `com.mysql.jdbc.Driver`；如果 MySQL 8.1+，[[Maven]] 坐标改为 `com.mysql:mysql-connector-j`。

```xml
<dependencies>
    <!-- MyBatis 核心 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.13</version>
    </dependency>

    <!-- MySQL 驱动：groupID 从 MySQL 8.1 起改为 com.mysql，artifact 改为 mysql-connector-j -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.33</version>
        <scope>runtime</scope>  <!-- runtime 表示编译时不需要，运行时才用 -->
    </dependency>

    <!-- JUnit 测试 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>test</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>8</source>
                <target>8</target>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
    </plugins>
</build>
```

---

### 第三步：[[mybatis-config.xml]] 核心配置

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--
        为什么要有 properties？
        数据库密码写死在 XML 里不安全，传到 Git 就泄露了。
        用外部 properties 文件 + 占位符 ${}，密码只在本地文件里。
    -->
    <properties resource="db.properties"/>

    <settings>
        <!--
            驼峰自动映射
            为什么开？数据库习惯用下划线（create_time），Java 习惯驼峰（createTime）。
            开这个开关后，MyBatis 自动把 create_time → createTime，不用你写 ResultMap。
            如果你没开，那就要手动 @Result(column="create_time", property="createTime")。
        -->
        <setting name="mapUnderscoreToCamelCase" value="true"/>

        <!--
            控制台打印 SQL 日志
            为什么开？新手 90% 的 bug 看打印的 SQL 一眼就能发现：
            - 参数传错了？看 ? 的值
            - SQL 语法错了？把打印的 SQL 复制到 Navicat 跑一遍
        -->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <typeAliases>
        <!--
            实体类别名：配置后写 mapper 时可以用 User 代替 com.demo.entity.User
            不配也行，就是每个地方都要写全限定类名，手累。
        -->
        <package name="com.demo.entity"/>
    </typeAliases>

    <!--
        数据库环境配在这里
        default="development" 表示默认用 development 这套配置
        你可以配多套：development（开发）、test（测试）、production（生产），切换 default 就行
    -->
    <environments default="development">
        <environment id="development">
            <!--
                transactionManager 事务管理器
                JDBC：手动控制提交/回滚（适合学习）
                MANAGED：让容器管理事务（适合 Spring 整合）
            -->
            <transactionManager type="JDBC"/>

            <!--
                dataSource 连接池
                POOLED：用的连接池，每次拿连接不用新建，用完放回去（推荐）
                UNPOOLED：每次新建连接，慢
                JNDI：从 Tomcat 等容器拿连接池（生产环境常见）
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 扫描整个包下的 Mapper 接口（注解方式） -->
        <package name="com.demo.mapper"/>
    </mappers>
</configuration>
```

配套的 `db.properties`（放在 `src/main/resources/` 下）：

```properties
# 不要把这个文件提交到 Git！（加到 .gitignore 里）
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis_demo?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai&useSSL=false&allowPublicKeyRetrieval=true
jdbc.username=root
jdbc.password=你的密码
```

**URL 参数详解（面试可能问）：**

| 参数 | 作用 | 不加的后果 |
|------|------|-----------|
| `useUnicode=true` | 启用 Unicode 编码 | 中文存成 `???` |
| `characterEncoding=utf-8` | 指定用 UTF-8 | 中文乱码 |
| `serverTimezone=Asia/Shanghai` | 设置时区为东八区 | 时间差 8 小时 |
| `useSSL=false` | 关闭 SSL 加密（开发环境） | 开发环境多此一举 |
| `allowPublicKeyRetrieval=true` | 允许获取公钥（MySQL 8.0+） | 连不上报 `Public Key Retrieval` 错误 |

---

### 第四步：实体类（[[JavaBean]]）

```java
package com.demo.entity;

import java.util.Date;

public class User {
    // 数据库字段一一对应。驼峰开启后，create_time 自动匹配 createTime
    private Integer id;
    private String username;
    private Integer age;
    private String email;
    private Date createTime;

    // 【必须】无参构造 —— MyBatis 反射创建对象时用 Class.newInstance() 调它
    public User() {}

    // getter/setter —— MyBatis 用 setter 注入查询结果，用 getter 获取参数值
    public Integer getId() { return id; }
    public void setId(Integer id) { this.id = id; }
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }
    public Integer getAge() { return age; }
    public void setAge(Integer age) { this.age = age; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
    public Date getCreateTime() { return createTime; }
    public void setCreateTime(Date createTime) { this.createTime = createTime; }

    @Override
    public String toString() {
        return "User{id=" + id + ", username='" + username + "', age=" + age + "}";
    }
}
```

---

### 第五步：工具类（[[SqlSessionFactory]]）

```java
package com.demo.util;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import java.io.InputStream;

public class MyBatisUtil {
    // SqlSessionFactory 是线程安全的，整个应用只创建一次
    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            // Resources.getResourceAsStream() 从 classpath 根目录找文件
            InputStream inputStream = Resources.getResourceAsStream("mybatis-config.xml");
            // Builder 模式：用配置文件构建 SqlSessionFactory
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (Exception e) {
            // 初始化失败时抛 RuntimeException，项目启动就能发现问题
            throw new RuntimeException("MyBatis 初始化失败", e);
        }
    }

    // false = 关闭自动提交，增删改后需要手动 session.commit()
    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession(false);
    }

    // true = 打开自动提交，每执行一次 SQL 自动 commit（查询用这个方便）
    public static SqlSession getSqlSession(boolean autoCommit) {
        return sqlSessionFactory.openSession(autoCommit);
    }
}
```

---

### 第六步：Mapper（两种写法都给你）

#### 方式 A：注解式（推荐简单 SQL）

```java
package com.demo.mapper;

import com.demo.entity.User;
import org.apache.ibatis.annotations.*;
import java.util.List;

public interface UserMapper {
    // @Select 里的 SQL 就是这个方法的 SQL
    @Select("SELECT * FROM user")
    List<User> selectAll();

    // #{id} = 占位符，MyBatis 自动用 PreparedStatement 的 ? 替换
    // 为什么不用 + 拼接？因为 #{id} 防 SQL 注入，+ 拼接会被 SQL 注入攻击
    @Select("SELECT * FROM user WHERE id = #{id}")
    User selectById(Integer id);

    // 多参数必须用 @Param 绑定名字，否则 MyBatis 不认识 #{xxx}
    @Select("SELECT * FROM user WHERE username=#{username} AND age=#{age}")
    List<User> selectByCondition(@Param("username") String username,
                                 @Param("age") Integer age);

    // 返回值 int 表示影响的行数
    @Insert("INSERT INTO user(username,age,email) VALUES(#{username},#{age},#{email})")
    int insert(User user);

    @Update("UPDATE user SET username=#{username},age=#{age},email=#{email} WHERE id=#{id}")
    int update(User user);

    @Delete("DELETE FROM user WHERE id=#{id}")
    int delete(Integer id);
}
```

#### 方式 B：[[XML]] 式（适合复杂 SQL、动态 SQL）

```java
// UserMapper.java — 只写方法签名，SQL 写在 XML 里
public interface UserMapper {
    // namespace + 方法名 = XML 中 sql 的唯一标识
    List<User> selectAll();
    User selectById(Integer id);
    int insert(User user);
}
```

配套的 `UserMapper.xml`（放在 `src/main/resources/com/demo/mapper/` 下）：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace = 接口的全限定类名
    MyBatis 根据这个 namespace 找到对应的接口
-->
<mapper namespace="com.demo.mapper.UserMapper">

    <!--
        id = 接口中的方法名
        resultType = 返回类型（因为配了 typeAliases，所以可以用 User）
    -->
    <select id="selectAll" resultType="User">
        SELECT * FROM user
    </select>

    <!--
        parameterType = 参数类型（可以省略，MyBatis 能自动推断）
        #{id} 和注解一样，预编译占位符
    -->
    <select id="selectById" resultType="User">
        SELECT * FROM user WHERE id = #{id}
    </select>

    <!--
        useGeneratedKeys + keyProperty：
        插入后自动把自增 ID 赋值给 user.id，这样你 insert 完就能拿到新 ID
    -->
    <insert id="insert" useGeneratedKeys="true" keyProperty="id">
        INSERT INTO user(username, age, email)
        VALUES(#{username}, #{age}, #{email})
    </insert>

    <!-- 动态 SQL 示例（<if> 标签拼接条件） -->
    <select id="selectByCondition" resultType="User">
        SELECT * FROM user WHERE 1=1
        <if test="username != null and username != ''">
            AND username LIKE CONCAT('%', #{username}, '%')
        </if>
        <if test="age != null">
            AND age = #{age}
        </if>
    </select>

</mapper>
```

> 用了 XML 式，要在 `[[mybatis-config.xml]]` 中注册：
> ```xml
> <mappers>
>     <mapper resource="com/demo/mapper/UserMapper.xml"/>
> </mappers>
> ```
> 注解式和 XML 式可以混用，但同一个方法不能同时用两种方式。

---

## 实战案例

### 完整测试类（复制就能跑）

```java
package com.demo;

import com.demo.entity.User;
import com.demo.mapper.UserMapper;
import com.demo.util.MyBatisUtil;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import java.util.List;

public class MyBatisTest {

    // 1. 查所有用户
    @Test
    public void testSelectAll() {
        // true = 自动提交（查询不需要事务，所以用 true 省事）
        try (SqlSession session = MyBatisUtil.getSqlSession(true)) {
            UserMapper mapper = session.getMapper(UserMapper.class);
            List<User> userList = mapper.selectAll();
            for (User u : userList) {
                System.out.println(u);
            }
        } // try-with-resources 自动 session.close()，不用手动关
    }

    // 2. 查单个用户
    @Test
    public void testSelectById() {
        try (SqlSession session = MyBatisUtil.getSqlSession(true)) {
            UserMapper mapper = session.getMapper(UserMapper.class);
            User u = mapper.selectById(1);
            System.out.println(u);
        }
    }

    // 3. 新增用户
    @Test
    public void testInsert() {
        // 不传 true = 手动提交。为什么？新增是写操作，万一出错了可以回滚
        try (SqlSession session = MyBatisUtil.getSqlSession()) {
            UserMapper mapper = session.getMapper(UserMapper.class);
            User user = new User();
            user.setUsername("嘎嘎");
            user.setAge(31);
            user.setEmail("gaga@163.com");
            int rows = mapper.insert(user);
            System.out.println("新增了 " + rows + " 行，新 ID = " + user.getId());
            session.commit(); // ⚠️ 不 commit 数据库里没数据！
        }
    }

    // 4. 修改用户（先查再改经典模式）
    @Test
    public void testUpdate() {
        try (SqlSession session = MyBatisUtil.getSqlSession(true)) {
            UserMapper mapper = session.getMapper(UserMapper.class);
            User u = mapper.selectById(1);  // 先查到
            u.setUsername("新名字");          // 再改字段
            u.setAge(99);
            int rows = mapper.update(u);     // 执行更新
            System.out.println("修改了 " + rows + " 行");
        }
    }

    // 5. 删除用户
    @Test
    public void testDelete() {
        try (SqlSession session = MyBatisUtil.getSqlSession(true)) {
            UserMapper mapper = session.getMapper(UserMapper.class);
            int rows = mapper.delete(2);
            System.out.println("删除了 " + rows + " 行");
        }
    }
}
```

### 项目完整结构

```
mybatis-demo/
├── pom.xml
└── src/
    ├── main/
    │   ├── java/com/demo/
    │   │   ├── entity/User.java
    │   │   ├── mapper/UserMapper.java
    │   │   ├── mapper/UserMapper.xml    ← XML 方式
    │   │   └── util/MyBatisUtil.java
    │   └── resources/
    │       ├── mybatis-config.xml
    │       └── db.properties            ← 数据库密码（不要提交 Git！）
    └── test/java/com/demo/
        └── MyBatisTest.java
```

---

## 踩坑记录

> [!warning] 坑1：增删改后数据没变（事务没提交）
> **现象**：`insert()` 返回了 1，但查数据库发现没数据。
> **原因**：`openSession()` 默认关闭自动提交。`insert/update/delete` 执行后数据还在「临时区」，必须 `commit()` 才能写入。
> **解决**：
> ```java
> // 方案A：手动提交
> SqlSession session = factory.openSession(false); // 默认
> session.insert(...);
> session.commit();  // 别忘了这行！
>
> // 方案B：自动提交（适合简单场景）
> SqlSession session = factory.openSession(true);  // 自动提交
> ```
> **原理**：[[MySQL]] [[InnoDB]] 引擎默认开启事务，不 commit 的话，其他连接看不到数据，数据库重启后数据也会丢失。

> [!warning] 坑2：SQL 注入风险（用了 `${}` 而不是 `#{}`）
> **现象**：用户输入 `' OR 1=1 --` 后，查出了所有数据。
> **原因**：`${}` 是字符串拼接，用户输入的内容直接拼进 SQL。
> ```java
> // ❌ 危险写法
> @Select("SELECT * FROM user WHERE username = '${name}'")
> // 用户输入：' OR 1=1 --
> // 变成：SELECT * FROM user WHERE username = '' OR 1=1 --'
> // -- 是 SQL 注释，后面的内容被注释掉了，结果查出了所有人！
>
> // ✅ 安全写法
> @Select("SELECT * FROM user WHERE username = #{name}")
> // #{name} 用 PreparedStatement 的 ? 占位，传入的值被当字符串处理，不会破坏 SQL 结构
> ```
> **原则**：永远用 `#{}` 预编译。`${}` 只在动态表名/列名时用（比如 `ORDER BY ${column}`），且必须是可控的代码内部值，不能来自用户输入。

> [!warning] 坑3：多参数没加 `@Param` 报错
> **现象**：`org.apache.ibatis.binding.BindingException: Parameter 'xxx' not found.`
> **原因**：多参数时 [[MyBatis]] 不知道 `#{xxx}` 对应哪个参数。
> ```java
> // ❌ 报错！两个参数 MyBatis 分不清
> List<User> select(String username, Integer age);
>
> // ✅ 正确：@Param 告诉 MyBatis 参数名
> List<User> select(@Param("username") String username, @Param("age") Integer age);
>
> // ✅ 或者用 Map（参数多时推荐）
> List<User> select(Map<String, Object> params);
> // map.put("username", "张三"); map.put("age", 22);
> ```
> **记住**：单参数不用 `@Param`，多参数必须加，对象参数（如 `User`）不用加（字段名自动匹配 `#{}`）。

> [!warning] 坑4：实体类没有无参构造
> **现象**：`java.lang.NoSuchMethodException: com.demo.entity.User.<init>()`
> **原因**：[[MyBatis]] 用 `Class.newInstance()` 反射创建对象，这个方法调的是**无参构造**。如果你写了有参构造没写无参构造，Java 不再提供默认无参构造。
> ```java
> // ❌ 会报错
> public User(String name) { this.username = name; }
> // 没有写无参构造 → MyBatis 无法创建对象
>
> // ✅ 要这样
> public User() {}  // 显式写出无参构造
> public User(String name) { this.username = name; }
> ```

> [!warning] 坑5：XML Mapper 没注册
> **现象**：`org.apache.ibatis.binding.BindingException: Invalid bound statement (not found)`
> **原因**：XML 文件不在 [[MyBatis]] 扫描路径内，或者没在配置中注册。
> ```xml
> <!-- 确保 mybatis-config.xml 中有这个： -->
> <mappers>
>     <!-- 注解方式用 package -->
>     <package name="com.demo.mapper"/>
>     <!-- XML 方式要单独注册（如果用 XML 就不要用上面的 package） -->
>     <mapper resource="com/demo/mapper/UserMapper.xml"/>
> </mappers>
> ```
> **另外注意**：XML 文件要放在和接口**同包同名**的位置，否则即使注册了也找不到。

> [!warning] 坑6：数据库连接失败 — Public Key Retrieval 错误
> **现象**：`CachingSha2PasswordPlugin` 或 `Public Key Retrieval is not allowed`
> **原因**：MySQL 8.0+ 默认用 `caching_sha2_password` 认证插件，需要获取公钥。
> **解决**：在 JDBC URL 末尾加 `allowPublicKeyRetrieval=true`。或者把 MySQL 用户改回 `mysql_native_password` 认证方式。

---

## 拓展延伸

### [[MyBatis]] vs [[JDBC]] 对比

| 对比项 | [[JDBC]] | [[MyBatis]] |
|--------|----------|-------------|
| 代码量 | 多（连接/语句/结果集/关闭 各写一遍） | 少（只写 SQL + 映射） |
| SQL 与 Java 耦合 | 硬耦合在代码里 | 分离到注解或 XML |
| 结果映射 | 手动 `rs.getXxx()` 逐字段 | 自动 + 驼峰 + `@Results` |
| 参数传递 | `ps.setXxx()` 按索引 | `#{}` 按名字，自动匹配 |
| 连接管理 | 手动 `close()` | 连接池 + try-with-resources |
| 性能优化 | 自己写连接池 | 内置一级/二级缓存 |
| 复杂 SQL | 写起来一样 | 支持动态 SQL（`<if>/<where>/<foreach>`） |
| 学习成本 | 低（原生） | 中（需要理解代理+映射机制） |

### 生产环境注意事项

1. **连接池调优**：`POOLED` 默认最大连接池大小是 10。高并发时不够，在 `[[mybatis-config.xml]]` 的 `<dataSource>` 里加：
   ```xml
   <property name="poolMaximumActiveConnections" value="20"/>
   <property name="poolMaximumIdleConnections" value="5"/>
   ```

2. **密码别写死在配置里**：用 `db.properties` 分离配置，在 `.gitignore` 里忽略它。生产环境用环境变量或配置中心（如 [[Nacos]]、[[Apollo]]）。

3. **SQL 日志脱敏**：`STDOUT_LOGGING` 会打印完整 SQL 和参数。生产环境用 [[Logback]] + 过滤，避免把用户手机号、密码打印到日志里。

4. **二级缓存脏读**：[[MyBatis]] 二级缓存是 Mapper 级别的。如果你的多个 Mapper 操作同一张表，一个 Mapper 更新了数据，另一个 Mapper 的缓存还是旧数据。**高并发场景慎用二级缓存，或者手动调 `flushCache=true`。**

5. **Mapper 命名规范**：
   - 插入：`insert` / `insertBatch`
   - 删除：`delete` / `deleteById`
   - 修改：`update` / `updateById`
   - 查询：`selectById` / `selectList` / `selectPage`
   - 方法名自解释，不要 `cx`、`xg` 之类的拼音缩写

### 学习路线

- 本篇 → [[mybatis/MyBatis关联查询|关联查询（@One/@Many）]] → [[mybatis/MyBatis建表脚本|完整建表脚本]]
- 然后 → [[../../02-Java进阶/并发编程核心知识|并发编程]] 和 [[../../04-JVM/index|JVM]]
- 实战 → [[../Spring Boot核心|Spring Boot 整合 MyBatis]]
- 配套数据库 → [[../../01-Java基础/数据类型|MySQL 数据类型]]

---

## 知识依赖图

先掌握 [[../../01-Java基础/面向对象三大特性|Java OOP]] · [[../../01-Java基础/异常处理|异常处理]] · [[../../02-Java进阶/集合框架总览|集合]] · [[../../01-Java基础/数据类型|MySQL基础]] → 再学本篇 [[MyBatis]] → 延伸 [[mybatis/MyBatis关联查询|关联查询]] · [[../Spring Boot核心|Spring Boot整合]] · [[../../02-Java进阶/并发编程核心知识|连接池原理]]

---

#技术/框架/MyBatis #技术/Java/数据库 #技术/数据库/MySQL #笔记/进阶之路
