---
title: MyBatis-Plus 从零入门
created: 2026-06-09
source: mp教程 + mybatis_plus 教程
tags: [技术/框架/MyBatis, 技术/框架/MyBatis-Plus, 笔记/进阶之路]
---

# [[MyBatis-Plus]]（MP）从零入门

## 核心概念

### [[MyBatis-Plus]] 是什么

MyBatis-Plus（简称 MP）是 [[MyBatis]] 的增强工具，**只做增强不做改变**——你用 MyBatis 的习惯全部保留，MP 在此基础上加了「自动 CRUD」和「条件构造器」。

### MP vs MyBatis vs JDBC

| 对比 | [[JDBC]] | [[MyBatis]] | [[MyBatis-Plus]] |
|------|----------|-------------|------------------|
| 基础 CRUD | 手写全套 SQL | 手写 SQL 映射 | **自动**（继承 BaseMapper） |
| 条件查询 | 手写 WHERE | 手写 SQL/XML | **QueryWrapper 链式构造** |
| 分页 | 手写 LIMIT | 手写或插件 | **内置分页插件** |
| 代码量 | 100 行 | 30 行 | 5 行 |

### MP 核心三件套

1. **`BaseMapper<T>`** — 继承后自带 CRUD 方法，不用写 SQL
2. **`@TableName / @TableId / @TableField`** — 实体类注解，配置表映射
3. **`QueryWrapper`** — 链式条件构造器，替代手写 WHERE

---

## 语法格式

### 一、Maven 依赖

```xml
<!-- MyBatis-Plus 核心（非 SpringBoot 版要加 core + annotation 两个） -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-core</artifactId>
    <version>3.5.5</version>
</dependency>
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-annotation</artifactId>
    <version>3.5.5</version>
</dependency>
<!-- MySQL 驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
    <scope>runtime</scope>
</dependency>
```

> **关键区别**：原生 MyBatis 用 `mybatis` 坐标，MP 用 `mybatis-plus-core` + `mybatis-plus-annotation`。SpringBoot 版只需要一个 `mybatis-plus-boot-starter`。

### 二、mybatis-config.xml（和 MyBatis 几乎一样）

```xml
<configuration>
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    <typeAliases>
        <package name="com.demo.entity"/>
    </typeAliases>
    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis_anno_demo?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai&useSSL=false&allowPublicKeyRetrieval=true"/>
                <property name="username" value="root"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <package name="com.demo.mapper"/>
    </mappers>
</configuration>
```

### 三、工具类（关键区别：用 MybatisSqlSessionFactoryBuilder）

```java
package com.demo.util;

import com.baomidou.mybatisplus.core.MybatisSqlSessionFactoryBuilder;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import java.io.InputStream;

public class MyBatisPlusUtil {
    private static SqlSessionFactory factory;

    static {
        try {
            InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
            // ⚠️ 这里必须用 MP 的 Builder，不能用 MyBatis 原生的
            factory = new MybatisSqlSessionFactoryBuilder().build(is);
        } catch (Exception e) {
            throw new RuntimeException("MP 初始化失败", e);
        }
    }

    public static SqlSession getSqlSession(boolean autoCommit) {
        return factory.openSession(autoCommit);
    }
    public static SqlSession getSqlSession() {
        return factory.openSession(false);
    }
}
```

### 四、实体类（MP 注解）

```java
package com.demo.entity;

import com.baomidou.mybatisplus.annotation.*;
import java.util.Date;

// @TableName：指定数据库表名（类名和表名不一致时必须加）
@TableName("user")
public class User {
    // @TableId：主键策略
    // IdType.AUTO = 数据库自增，IdType.ASSIGN_ID = 雪花算法（默认）
    @TableId(type = IdType.AUTO)
    private Integer id;
    private String username;
    private Integer age;
    private String email;

    // @TableField：字段映射（驼峰已开启时可省略，这里显式写上）
    @TableField("create_time")
    private Date createTime;

    // 非数据库字段必须加 exist=false，否则 MP 会误当成表字段报错
    @TableField(exist = false)
    private String extraField;

    // getter/setter/toString...
}
```

### 五、Mapper（核心优势：继承 BaseMapper）

```java
package com.demo.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.demo.entity.User;

// 继承 BaseMapper<User> → 自带 CRUD 方法，一行代码都不用写
public interface UserMapper extends BaseMapper<User> {
    // 只需要写自定义查询方法（关联查询等）
}
```

**BaseMapper 自带的方法（约 20+ 个，最常用的）：**

| 方法 | 作用 |
|------|------|
| `insert(T entity)` | 新增 |
| `deleteById(id)` | 按 ID 删除 |
| `updateById(T entity)` | 按 ID 修改 |
| `selectById(id)` | 按 ID 查询 |
| `selectList(QueryWrapper)` | 条件查询列表 |
| `selectOne(QueryWrapper)` | 查询单条 |
| `selectCount(QueryWrapper)` | 查询总数 |
| `selectPage(Page, QueryWrapper)` | 分页查询 |

---

## 实战案例

### 1. 基础 CRUD

```java
public class AppTest {

    // 查询单个
    @Test
    public void testSelectById() {
        try (SqlSession session = MyBatisPlusUtil.getSqlSession(true)) {
            UserMapper mapper = session.getMapper(UserMapper.class);
            User user = mapper.selectById(1);  // MP 自带的
            System.out.println(user);
        }
    }

    // 新增（自动回填自增 ID）
    @Test
    public void testInsert() {
        try (SqlSession session = MyBatisPlusUtil.getSqlSession(true)) {
            UserMapper mapper = session.getMapper(UserMapper.class);
            User u = new User();
            u.setUsername("MP 新增");
            u.setAge(22);
            int row = mapper.insert(u);
            System.out.println("新 ID = " + u.getId());  // 插入后自动回填
        }
    }

    // 查询全部
    @Test
    public void testSelectAll() {
        try (SqlSession session = MyBatisPlusUtil.getSqlSession(true)) {
            UserMapper mapper = session.getMapper(UserMapper.class);
            List<User> list = mapper.selectList(null);  // null = 无条件
            list.forEach(System.out::println);
        }
    }

    // 修改（按 ID）
    @Test
    public void testUpdate() {
        try (SqlSession session = MyBatisPlusUtil.getSqlSession(true)) {
            UserMapper mapper = session.getMapper(UserMapper.class);
            User u = mapper.selectById(1);
            u.setUsername("新名字");
            mapper.updateById(u);  // 按 ID 更新非空字段
        }
    }

    // 删除
    @Test
    public void testDelete() {
        try (SqlSession session = MyBatisPlusUtil.getSqlSession(true)) {
            UserMapper mapper = session.getMapper(UserMapper.class);
            mapper.deleteById(1);
        }
    }
}
```

### 2. QueryWrapper 条件查询（MP 核心利器！）

```java
// 统一准备
QueryWrapper<User> wrapper = new QueryWrapper<>();
UserMapper mapper = session.getMapper(UserMapper.class);
```

| 场景 | 代码 |
|------|------|
| **等值查询** | `wrapper.eq("username", "张三")` |
| **不等查询** | `wrapper.ne("age", 22)` |
| **大于/小于** | `wrapper.gt("age", 22)` / `.lt("age", 30)` |
| **范围查询** | `wrapper.between("age", 22, 28)` |
| **模糊查询（中%）** | `wrapper.like("username", "张")` |
| **左模糊（%后缀）** | `wrapper.leftLike("email", "qq.com")` |
| **右模糊（前缀%）** | `wrapper.rightLike("username", "张")` |
| **包含** | `wrapper.in("age", 22, 25, 30)` |
| **排除** | `wrapper.notIn("id", 1, 2)` |
| **为空/非空** | `wrapper.isNull("email")` / `.isNotNull("email")` |
| **排序** | `wrapper.orderByDesc("age").orderByAsc("id")` |
| **指定字段** | `wrapper.select("username", "age")` |
| **组合条件** | `wrapper.and(w -> w.eq(...)).or(w -> w.gt(...))` |
| **动态条件** | `wrapper.like(name != null, "username", name)` |

**动态条件是企业最常用模式：**

```java
// 前端传参，非空才拼接，空参数自动忽略
String username = "张";   // 可能为 null
Integer age = null;       // 可能为 null

QueryWrapper<User> wrapper = new QueryWrapper<>();
wrapper.like(username != null, "username", username)  // 不为空才加条件
       .eq(age != null, "age", age);
```

**分页查询：**

```java
// MP 分页：用 .last() 拼接 LIMIT
List<User> list = mapper.selectList(
    new QueryWrapper<User>().gt("age", 20).last("LIMIT 0,2")
);
```

### 3. 关联查询（兼容 MyBatis 原生注解）

**用法和原生 MyBatis 一样**，见 [[mybatis/MyBatis关联查询|MyBatis 关联查询]]。唯一区别是在 User 实体中，关联属性要加 `@TableField(exist = false)`：

```java
@TableField(exist = false)
private UserInfo userInfo;       // 一对一

@TableField(exist = false)
private List<Order> orderList;   // 一对多

@TableField(exist = false)
private List<Role> roleList;     // 多对多
```

---

## 踩坑记录

> [!warning] 工具类必须用 MP 的 SqlSessionFactoryBuilder
> **现象**：`selectList(null)` 报错，提示找不到 BaseMapper 方法
> **原因**：用了 MyBatis 原生的 `SqlSessionFactoryBuilder`，它不认识 MP 的 `BaseMapper`。
> **解决**：换成 `com.baomidou.mybatisplus.core.MybatisSqlSessionFactoryBuilder`

> [!warning] 非数据库字段没加 @TableField(exist = false)
> **现象**：`Invalid column 'extraField'` SQL 异常
> **原因**：MP 默认认为所有字段都是表字段，实体类中多了属性但表里没有对应列就会报错。
> **解决**：非表字段全部加上 `@TableField(exist = false)`

> [!warning] 表名是 MySQL 关键字要转义
> **现象**：`order` 表操作报 SQL 语法错误
> **原因**：`order` 是 MySQL 关键字，不能直接当表名。
> **解决**：`@TableName("\`order\`")` 用反引号转义

> [!warning] QueryWrapper 字段名是字符串，改字段名会忘改这里
> **现象**：改了实体类字段名但没改 QueryWrapper 里的字符串，运行时才报错。
> **建议**：用 `LambdaQueryWrapper` 替代，它用方法引用，编译期就能发现问题：
> ```java
> LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
> wrapper.eq(User::getUsername, "张三");  // 类型安全，改字段名会自动编译错误
> ```

---

## 拓展延伸

### MP 分页插件（生产推荐）

MP 内置 `PaginationInnerInterceptor`，配合 `Page` 对象实现分页：

```java
// 配置分页插件（在 mybatis-config.xml 中）
// 或在 SpringBoot 中 @Bean 配置

Page<User> page = new Page<>(1, 10);  // 第1页，每页10条
Page<User> result = mapper.selectPage(page, null);
System.out.println(result.getRecords());   // 数据列表
System.out.println(result.getTotal());     // 总记录数
System.out.println(result.getPages());     // 总页数
```

### MyBatis vs MyBatis-Plus 选择

| 场景 | 选哪个 |
|------|--------|
| 简单 CRUD 为主 | **MP**（少写 80% 代码） |
| 复杂 SQL、动态 SQL 多 | MyBatis（MP 也能兼容） |
| 新项目 | **MP**（增强不改变，可降级回 MyBatis） |
| 老项目改造 | MP（兼容原有所有 MyBatis 代码） |

---

## 知识依赖图

先掌握 [[mybatis/MyBatis从零入门|MyBatis 基础]] · [[mybatis/MyBatis关联查询|MyBatis 关联查询]] → 再学本篇 MP → 延伸 [[../../../Java SE 考点复习/14-集合框架|Spring Boot 整合 MP]] · 分页插件 · 代码生成器

---

#技术/框架/MyBatis #技术/框架/MyBatis-Plus #笔记/进阶之路
