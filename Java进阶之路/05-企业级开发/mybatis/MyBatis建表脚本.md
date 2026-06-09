---
created: 2026-06-09
source: mybatis教程
tags: [java, mybatis, sql, 数据库]
---

# MyBatis 建表脚本（4表 + 中间表）

> 配套笔记：[[mybatis/MyBatis从零入门]] · [[mybatis/MyBatis关联查询]]

```sql
-- 创建数据库
CREATE DATABASE IF NOT EXISTS mybatis_anno_demo
    DEFAULT CHARACTER SET utf8mb4;
USE mybatis_anno_demo;

-- 1. 用户表（基础 CRUD）
CREATE TABLE `user` (
    `id` INT PRIMARY KEY AUTO_INCREMENT,
    `username` VARCHAR(50) NOT NULL,
    `age` INT,
    `email` VARCHAR(50),
    `create_time` DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- 2. 订单表（一对多：一个用户 → 多个订单）
CREATE TABLE `order` (
    `id` INT PRIMARY KEY AUTO_INCREMENT,
    `order_no` VARCHAR(50) NOT NULL,
    `order_price` DECIMAL(10,2) NOT NULL,
    `user_id` INT,
    `create_time` DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (`user_id`) REFERENCES `user`(`id`)
);

-- 3. 用户详情表（一对一：一个用户 → 一条详情）
CREATE TABLE `user_info` (
    `id` INT PRIMARY KEY AUTO_INCREMENT,
    `user_id` INT UNIQUE,
    `phone` VARCHAR(20),
    `address` VARCHAR(200),
    FOREIGN KEY (`user_id`) REFERENCES `user`(`id`)
);

-- 4. 角色表
CREATE TABLE `role` (
    `id` INT PRIMARY KEY AUTO_INCREMENT,
    `role_name` VARCHAR(50) NOT NULL,
    `role_desc` VARCHAR(200)
);

-- 5. 用户-角色中间表（多对多）
CREATE TABLE `user_role` (
    `id` INT PRIMARY KEY AUTO_INCREMENT,
    `user_id` INT,
    `role_id` INT,
    FOREIGN KEY (`user_id`) REFERENCES `user`(`id`),
    FOREIGN KEY (`role_id`) REFERENCES `role`(`id`)
);

-- 插入测试数据
INSERT INTO `user` (username, age, email) VALUES
('张三', 22, 'zhangsan@163.com'),
('李四', 25, 'lisi@163.com');

INSERT INTO `order` (order_no, order_price, user_id) VALUES
('ORDER_001', 100.00, 1),
('ORDER_002', 200.00, 1);

INSERT INTO `user_info` (user_id, phone, address) VALUES
(1, '13800138000', '北京市朝阳区');

INSERT INTO `role` (role_name, role_desc) VALUES
('管理员', '系统管理员'),
('普通用户', '普通业务用户');

INSERT INTO `user_role` (user_id, role_id) VALUES
(1, 1),
(1, 2);
```

---

#Java #MyBatis #SQL #建表
