---
title: Spring Boot 核心
created: 2026-06-08
source: https://javabetter.cn/springboot/
tags: [技术/框架/Spring, 技术/Java/企业级, 笔记/进阶之路]
---

# [[Spring Boot]] 核心

## 核心概念

### 核心特性

| 特性 | 说明 |
|------|------|
| 自动配置 | `@EnableAutoConfiguration` + spring.factories |
| 起步依赖 | `spring-boot-starter-web` 一键引入 |
| 嵌入式容器 | Tomcat/Jetty 内嵌，无需部署 |

```java
@SpringBootApplication = @Configuration + @EnableAutoConfiguration + @ComponentScan
```

### IoC（控制反转）

对象的创建和管理交给 Spring 容器，你只管用 `@Autowired` 取。

```java
@Component / @Service / @Repository / @Controller  // 组件标记
@Autowired      // 按类型注入
@Bean           // 配置类中声明 Bean
```

### AOP（面向切面编程）

在不改原代码的情况下添加日志、事务、权限等公共逻辑。

**底层**：有接口→JDK动态代理；无接口→CGLIB动态代理。

### Bean 生命周期

```
实例化 → 属性赋值 → 初始化前 → 初始化 → 初始化后 → 使用 → 销毁
```

### @Transactional 事务

```java
@Transactional(rollbackFor = Exception.class)
public void createUser(User user) {
    userDao.insert(user);
    // 抛异常自动回滚
}
```

**注意**：默认只回滚 `RuntimeException` 和 `Error`，受检异常不回滚。同类方法直接调用 `@Transactional` 失效。

## 踩坑记录

> [!warning] 事务失效场景
> 1. 同类方法直接调用（不走代理）
> 2. 非 `public` 方法
> 3. 捕获异常没抛出去

> [!warning] 循环依赖
> Spring 通过三级缓存解决 setter 注入的循环依赖，但构造器注入不行。

## 知识依赖图

先掌握 [[../../Java SE 考点复习/08-面向对象基础|OOP]] → 再学本篇 → 延伸 [[../mybatis/MyBatis从零入门|MyBatis 整合]]

---

#技术/框架/Spring #技术/Java/企业级 #笔记/进阶之路
