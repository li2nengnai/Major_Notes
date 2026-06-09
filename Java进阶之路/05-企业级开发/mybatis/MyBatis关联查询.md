---
created: 2026-06-09
source: mybatis教程 + mybatis_v1 项目
tags: [java, mybatis, orm, 数据库, 多表查询]
---

# MyBatis 进阶：关联查询与 `@Results` 映射

> 配套SQL建表脚本 → [[mybatis/MyBatis建表脚本|数据库初始化脚本]]

---

## 为什么需要关联查询？

之前的 CRUD 只操作 **一张表**（user），但真实项目的数据分散在多张表里：

```
user（用户）      order（订单）        role（角色）
┌──────┐       ┌──────────┐       ┌──────────┐
│ id   │ ←─── │ user_id  │       │ id       │
│ name │       │ order_no │       │ role_name│
└──────┘       └──────────┘       └──────────┘
     │                                ↑
     │          user_role（中间表）      │
     │       ┌─────────────────┐      │
     └────── │ user_id │ role_id│─────┘
             └─────────────────┘
```

- **一对一**：一个用户 → 一条详情（user → user_info）
- **一对多**：一个用户 → 多个订单（user → order）
- **多对多**：一个用户 → 多个角色，一个角色 → 多个用户（user ↔ role）

---

## 一、多参数查询（基础铺垫）

在学关联查询前，先掌握多参数传递：

### `@Param` 方式（推荐）

```java
@Select("SELECT * FROM user WHERE username=#{username} AND age=#{age}")
List<User> selectByCondition(@Param("username") String username,
                             @Param("age") Integer age);
```

### Map 方式

```java
@Select("SELECT * FROM user WHERE username=#{username} AND age=#{age}")
List<User> selectByMap(Map<String,Object> map);

// 调用时：
Map<String,Object> map = new HashMap<>();
map.put("username","张三");
map.put("age",22);
mapper.selectByMap(map);
```

**`@Param` vs Map：**

| 方式 | 适用场景 | 可读性 |
|------|---------|--------|
| `@Param` | 参数固定、少（≤5个） | ✅ 高 |
| Map | 参数不确定、多 | ❌ 低（不知道 key 是什么） |

---

## 二、`@Results` 自定义结果映射

默认情况下 MyBatis 自动把 **数据库字段 → 实体属性**（驼峰开启时 `create_time` → `createTime`）。

但遇到以下情况必须用 `@Results`：

- 字段名和属性名对不上（且驼峰也救不了）
- 需要关联查询（`@One` / `@Many`）
- 主键标记（`id = true` 提高性能）

```java
@Select("SELECT * FROM user WHERE id = #{id}")
@Results(value = {
    @Result(column = "id", property = "id", id = true),     // 主键
    @Result(column = "username", property = "username"),
    @Result(column = "age", property = "age"),
    @Result(column = "email", property = "email"),
    @Result(column = "create_time", property = "createTime"), // 驼峰映射（显式写出来）
})
User selectById(Integer id);
```

### `@Results` 语法拆解

```java
@Results({
    @Result(column = "数据库字段名", property = "实体属性名", id = true)
})
```

| 属性 | 作用 | 必填 |
|------|------|:----:|
| `column` | 数据库的列名 | ✅ |
| `property` | 实体类的属性名 | ✅ |
| `id` | 是否为主键（true 时 MyBatis 会缓存优化） | 仅主键 |
| `one` | 一对一关联（`@One`） | 关联查询时 |
| `many` | 一对多/多对多关联（`@Many`） | 关联查询时 |

---

## 三、一对一查询（用户 → 详情）

### 数据库表

```
user 表                  user_info 表
id  (主键)               id  (主键)
username                 user_id (关联 user.id)
age                      phone
email                    address
create_time
```

### 步骤1：实体类

```java
// UserInfo.java —— 用户详情
public class UserInfo {
    private Integer id;
    private Integer userId;
    private String phone;
    private String address;
    // getter/setter/toString ...
}

// User.java —— 添加 userInfo 属性
public class User {
    // ...原有字段...
    private UserInfo userInfo;    // 一对一：一个用户对应一条详情
    // getter/setter
}
```

### 步骤2：UserInfoMapper

```java
@Select("SELECT * FROM user_info WHERE user_id = #{userId}")
UserInfo selectUserInfoByUserId(Integer userId);
```

### 步骤3：UserMapper 中调用（核心！）

```java
@Select("SELECT * FROM user WHERE id = #{id}")
@Results({
    @Result(column = "id", property = "id", id = true),
    @Result(column = "username", property = "username"),
    @Result(column = "create_time", property = "createTime"),
    // ⭐ 一对一：把 user.id 传给 UserInfoMapper，结果赋值给 userInfo
    @Result(
        column = "id",                   // 当前查询结果中的 user.id
        property = "userInfo",           // 赋值给 User.userInfo 属性
        one = @One(select = "com.demo.mapper.UserInfoMapper.selectUserInfoByUserId")
    )
})
User selectUserWithInfo(Integer id);
```

**执行逻辑：**

```
1. 执行 SELECT * FROM user WHERE id = 1      → 查到用户
2. 看到 @One(select = "...UserInfoMapper...")
3. 拿 user.id (=1) 再去执行 SELECT * FROM user_info WHERE user_id = 1
4. 把结果封装成 UserInfo 对象 → 赋值给 User.userInfo
```

### 步骤4：测试

```java
@Test
public void testOneToOne() {
    try (SqlSession session = MyBatisUtil.getSqlSession(true)) {
        UserMapper mapper = session.getMapper(UserMapper.class);
        User user = mapper.selectUserWithInfo(1);
        System.out.println("用户：" + user);
        System.out.println("详情：" + user.getUserInfo());
    }
}
```

---

## 四、一对多查询（用户 → 订单）

### 数据库表

```
user 表                  order 表
id  (主键)               id  (主键)
username                 order_no
...                      order_price
                         user_id (关联 user.id)
```

### 步骤1：实体类

```java
// Order.java
public class Order {
    private Integer id;
    private String orderNo;
    private BigDecimal orderPrice;
    private Integer userId;
    private Date createTime;
    // getter/setter/toString ...
}

// User.java —— 添加 orderList 属性
public class User {
    // ...原有字段 + userInfo...
    private List<Order> orderList;    // 一对多：一个用户有多个订单
    // getter/setter
}
```

### 步骤2：OrderMapper

```java
@Select("SELECT * FROM `order` WHERE user_id = #{userId}")
List<Order> selectOrderByUserId(Integer userId);
```

### 步骤3：UserMapper 中调用

```java
@Select("SELECT * FROM user WHERE id = #{id}")
@Results({
    @Result(column = "id", property = "id", id = true),
    @Result(column = "username", property = "username"),
    @Result(column = "create_time", property = "createTime"),
    // ⭐ 一对多：把 user.id 传给 OrderMapper，结果赋值给 orderList
    @Result(
        property = "orderList",
        column = "id",
        many = @Many(
            select = "com.demo.mapper.OrderMapper.selectOrderByUserId",
            fetchType = FetchType.EAGER   // 立即加载（默认是 LAZY 懒加载）
        )
    )
})
User selectUserWithOrder(Integer id);
```

### `@One` vs `@Many` 区别

| 注解 | 返回类型 | 对应关系 |
|------|---------|---------|
| `@One` | 单个对象 | 一对一 |
| `@Many` | 集合（List） | 一对多 / 多对多 |

### `fetchType` 懒加载 vs 立即加载

```java
fetchType = FetchType.LAZY    // 懒加载：用到时才查（默认，省性能）
fetchType = FetchType.EAGER   // 立即加载：查主表时一口气全查出来
```

**什么区别？**

```
立即加载：查用户 → 马上查订单（不管你要不要订单数据）
懒加载：  查用户 → 只有你调 user.getOrderList() 时才去查订单
```

---

## 五、多对多查询（用户 → 角色）

### 数据库表

```
user 表              user_role（中间表）      role 表
id  (主键)           id  (主键)              id  (主键)
username             user_id (→ user.id)     role_name
...                  role_id (→ role.id)     role_desc
```

**多对多必须通过中间表拆成两个一对多。**

### 步骤1：实体类

```java
// Role.java
public class Role {
    private Integer id;
    private String roleName;
    private String roleDesc;
    // getter/setter/toString ...
}

// User.java —— 添加 roleList 属性
public class User {
    // ...原有字段 + userInfo + orderList...
    private List<Role> roleList;    // 多对多：一个用户有多个角色
    // getter/setter
}
```

### 步骤2：RoleMapper（通过中间表联查）

```java
// 多对多 = user → user_role → role，两次 JOIN
@Select("SELECT r.* FROM role r " +
        "INNER JOIN user_role ur ON r.id = ur.role_id " +
        "WHERE ur.user_id = #{userId}")
List<Role> selectRoleByUserId(Integer userId);
```

### 步骤3：UserMapper 中调用

```java
@Select("SELECT * FROM user WHERE id = #{id}")
@Results({
    @Result(column = "id", property = "id", id = true),
    @Result(column = "username", property = "username"),
    @Result(column = "create_time", property = "createTime"),
    // 多对多（和一对多一样用 @Many，只是 SQL 多了 JOIN）
    @Result(
        column = "id",
        property = "roleList",
        many = @Many(select = "com.demo.mapper.RoleMapper.selectRoleByUserId")
    )
})
User selectUserWithRole(Integer id);
```

---

## 六、全关联查询（一步到位）

把一对一 + 一对多 + 多对多全塞进一个方法：

```java
@Select("SELECT * FROM user WHERE id = #{id}")
@Results({
    @Result(column = "id", property = "id", id = true),
    @Result(column = "username", property = "username"),
    @Result(column = "create_time", property = "createTime"),
    // 一对一
    @Result(column = "id", property = "userInfo",
            one = @One(select = "com.demo.mapper.UserInfoMapper.selectUserInfoByUserId")),
    // 一对多
    @Result(column = "id", property = "orderList",
            many = @Many(select = "com.demo.mapper.OrderMapper.selectOrderByUserId")),
    // 多对多
    @Result(column = "id", property = "roleList",
            many = @Many(select = "com.demo.mapper.RoleMapper.selectRoleByUserId"))
})
User selectUserAllRelation(Integer id);
```

---

## 七、完整项目结构

```
mybatis-annotation-demo/
├── pom.xml
└── src/
    ├── main/java/com/demo/
    │   ├── entity/
    │   │   ├── User.java        ← 含 userInfo/orderList/roleList
    │   │   ├── UserInfo.java    ← 一对一
    │   │   ├── Order.java       ← 一对多
    │   │   └── Role.java        ← 多对多
    │   ├── mapper/
    │   │   ├── UserMapper.java  ← 核心：CRUD + 关联查询
    │   │   ├── UserInfoMapper.java
    │   │   ├── OrderMapper.java
    │   │   └── RoleMapper.java
    │   └── util/
    │       └── MyBatisUtil.java
    ├── resources/
    │   └── mybatis-config.xml
    └── test/java/com/
        └── BaseAnnoTest.java
```

---

## ⚠️ 常见坑

1. **`@Results` 不生效**：同一个方法上 `@Results` 和 `@Select` 必须成对出现，不能复用
2. **N+1 问题**：懒加载时循环查数据库（比如查 100 个用户 each 触发一次订单查询），考虑用 `fetchType = EAGER` 或 XML 联表
3. **`@One` 路径写错**：`select = "com.demo.mapper.UserInfoMapper.selectUserInfoByUserId"` — 包名+类名+方法名，**一个字符都不能错**
4. **主键忘记 `id = true`**：不加也能用，但 MyBatis 无法做缓存优化

---

#Java #MyBatis #ORM #关联查询 #一对多 #多对多
