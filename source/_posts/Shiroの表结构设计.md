---
title: Shiroの表结构设计
date: 2019-04-29 15:47:00
categories: Java
tags:
  - springboot
  - shiro
---
# Shiroの表结构设计
>   通常我们的权限设计都是 用户--角色--权限 ,其中角色是我们写代码的人没法控制的,它可以有多条权限,每个用户又可以设计为拥有多个角色.因此如果从角色着手进行权限验证,系统都必须根据用户的配置动起来,非常复杂.角色的作用其实只是用来管理分配权限的,真正的验证只验证权限。
## 1.表设计
### 1.1数据库结构
 -users 用户表
 -roles 角色表
 -permissions 权限表
 -users_roles 用户角色关联表
 -roles_permissions 角色权限关联表
## 2.创建表
### 2.1users表
![![524f7a79cf3bcbae.png](https://image.kalifun.top/upload/1904/524f7a79cf3bcbae.png)](https://image.kalifun.top/upload/1904/524f7a79cf3bcbae.png)
可能最关心的就是locked这个字段吧，它的类型为TINYINT。这个值只能是0、1.当我们使用mybatis时，获得这个类型的数据是0时会转为false。
### 2.2roles表
![![b3fcda6dc4900f46.png](https://image.kalifun.top/upload/1904/b3fcda6dc4900f46.png)](https://image.kalifun.top/upload/1904/b3fcda6dc4900f46.png)
pid表示父节点，就是说，当前的角色可能有上级节点，比如老师，这个角色可能就有父节点计科教师，如果存在父节点，这个字段值就是父级节点的ID，根据这个ID，在展示数据的时候就很方便的展示出其在哪个父节点下。
### 2.3users_roles表格
![![48955f65b0730ad9.png](https://image.kalifun.top/upload/1904/48955f65b0730ad9.png)](https://image.kalifun.top/upload/1904/48955f65b0730ad9.png)
 这张表主要描述指定用户与角色间的依赖关系。
### 2.4permissions表
![![f01be9f000a1a985.png](https://image.kalifun.top/upload/1904/f01be9f000a1a985.png)](https://image.kalifun.top/upload/1904/f01be9f000a1a985.png)
### 2.5roles_permissions表
![![527412228ce38f2c.png](https://image.kalifun.top/upload/1904/527412228ce38f2c.png)](https://image.kalifun.top/upload/1904/527412228ce38f2c.png)
主要描述角色和权限间的依赖关系
## 3.创建源码
```
-- --------------------------------------------------------
-- 主机:                           127.0.0.1
-- 服务器版本:                        10.3.10-MariaDB - mariadb.org binary distribution
-- 服务器操作系统:                      Win64
-- HeidiSQL 版本:                  9.4.0.5125
-- --------------------------------------------------------

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET NAMES utf8 */;
/*!50503 SET NAMES utf8mb4 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;


-- 导出 imgurl 的数据库结构
CREATE DATABASE IF NOT EXISTS `imgurl` /*!40100 DEFAULT CHARACTER SET latin1 */;
USE `imgurl`;

-- 导出  表 imgurl.permissions 结构
CREATE TABLE IF NOT EXISTS `permissions` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `permission` int(11) NOT NULL DEFAULT 0 COMMENT '权限编号',
  `description` int(11) NOT NULL DEFAULT 0 COMMENT '权限描述',
  `rid` int(11) NOT NULL DEFAULT 0 COMMENT '权限关联角色',
  `available` int(11) NOT NULL DEFAULT 0 COMMENT '是否锁定',
  PRIMARY KEY (`id`),
  UNIQUE KEY `users_roles_permission` (`permission`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

-- 数据导出被取消选择。
-- 导出  表 imgurl.roles 结构
CREATE TABLE IF NOT EXISTS `roles` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `role` varchar(50) NOT NULL DEFAULT '0' COMMENT '角色名称',
  `description` varchar(150) NOT NULL DEFAULT '0' COMMENT '角色描述',
  `pid` int(11) NOT NULL DEFAULT 0 COMMENT '父节点',
  `available` tinyint(4) NOT NULL DEFAULT 0 COMMENT '是否锁定',
  PRIMARY KEY (`id`),
  UNIQUE KEY `roles_role` (`role`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

-- 数据导出被取消选择。
-- 导出  表 imgurl.roles_permissions 结构
CREATE TABLE IF NOT EXISTS `roles_permissions` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `roles_id` int(11) DEFAULT 0 COMMENT '角色编号',
  `permissions_id` int(11) DEFAULT 0 COMMENT '权限编号',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

-- 数据导出被取消选择。
-- 导出  表 imgurl.users 结构
CREATE TABLE IF NOT EXISTS `users` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `username` varchar(50) NOT NULL DEFAULT '0' COMMENT '用户名',
  `password` varchar(50) NOT NULL DEFAULT '0' COMMENT '密码',
  `salt` varchar(50) NOT NULL DEFAULT '0' COMMENT '盐值',
  `role_id` varchar(50) NOT NULL DEFAULT '0' COMMENT '角色id',
  `locked` tinyint(1) NOT NULL DEFAULT 0 COMMENT '锁定',
  PRIMARY KEY (`id`),
  UNIQUE KEY `users_username` (`username`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

-- 数据导出被取消选择。
-- 导出  表 imgurl.users_roles 结构
CREATE TABLE IF NOT EXISTS `users_roles` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` int(20) NOT NULL DEFAULT 0 COMMENT '用户编号',
  `role_id` int(20) NOT NULL DEFAULT 0 COMMENT '角色编号',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

-- 数据导出被取消选择。
/*!40101 SET SQL_MODE=IFNULL(@OLD_SQL_MODE, '') */;
/*!40014 SET FOREIGN_KEY_CHECKS=IF(@OLD_FOREIGN_KEY_CHECKS IS NULL, 1, @OLD_FOREIGN_KEY_CHECKS) */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;

```
