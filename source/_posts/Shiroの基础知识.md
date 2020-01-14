---
title: Shiroの基础知识
date: 2019-05-08 16:41:00
categories: Java
tags:
  - springboot
---
# Shiroの基础知识
## Authenticator(用户认证)
> ### 用户认证，用户去访问系统，系统要验证用户身份的合法性
## 认证方法
 最常用的用户身份验证的方法：1、用户名密码方式、2、指纹打卡机、3、基于证书验证方法。。系统验证用户身份合法，用户方可访问系统的资源。
## 认证流程
-  创建一个简单的Realm（Realm 充当了 Shiro 与应用安全数据间的“桥梁”或者“连接器”。）
-  创建一个SecurityManager 
-  subject主体发送请求
-  用户名密码认证
-  Authenticator认证
-  Realm验证数据
## 测试Code
```
package com.imgurl;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.realm.SimpleAccountRealm;
import org.apache.shiro.subject.Subject;
import org.junit.Before;
import org.junit.Test;

public class Authrnrization {
//    创建realm
    SimpleAccountRealm simpleAccountRealm = new SimpleAccountRealm();

    @Before
    public void  add() {
        simpleAccountRealm.addAccount("admin","12345");
    }


    @Test
    public void checkauthrization(){
//        1.创建一个管理器
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        defaultSecurityManager.setRealm(simpleAccountRealm);

//        2.主体发动请求
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        Subject subject = SecurityUtils.getSubject();

//        3.认证
        UsernamePasswordToken token = new UsernamePasswordToken("admin","12345");
        subject.login(token);

        System.out.println("认证："+subject.isAuthenticated());
    }
}

```
## Authorizer(用户授权)
> 用户授权，简单理解为访问控制，在用户认证通过后，系统对用户访问资源进行控制，用户具有资源的访问权限方可访问。
## 授权流程
-  创建一个SecurityManager 
-  主体授权
-  SecurityManager授权
-  Authorizer授权
-  Realm获取用户角色权限数据
## 测试Code
```
package com.imgurl;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.realm.SimpleAccountRealm;
import org.apache.shiro.subject.Subject;
import org.junit.Before;
import org.junit.Test;

public class roles {
    SimpleAccountRealm simpleAccountRealm = new SimpleAccountRealm();
    @Before
    public void add() {
        simpleAccountRealm.addAccount("admin","12345","Manager","Otheruser");
    }

    @Test
    public void Authorizer(){
        //    1.创建securityManager
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        defaultSecurityManager.setRealm(simpleAccountRealm);

//        2.主体发送请求授权
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        Subject subject = SecurityUtils.getSubject();

//        3.authorizer授权
        UsernamePasswordToken token = new UsernamePasswordToken("admin","12345");
        subject.login(token);
        System.out.println("认证："+subject.isAuthenticated());
        subject.checkRoles("Manager","Otheruser");
    }
}
```
## Realm
> ###   Realm: 域，Shiro 从 Realm 中获取用户，角色，权限信息。可以把 Relam 看成 DataSource，即安全数据源。
 前面我们使用的是SimpleAccountRealm，并通过addAccount来预设用户信息。
### IniRealm
> IniRealm 顾名思义，即通过读取 .ini 文件来获取用户，角色，权限信息。
 创建xxx.ini文件(在Test目录下创建resource)
```
[users]
admin = 123456,Manager
fun = 123456,Otheruser
用户名 = 密码,角色
[roles]
Manager = user:delete,user:add,user:update,user:select
Otheruser = user:select
角色 = 权限
```
#### 测试Code
```
package com.imgurl;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.realm.text.IniRealm;
import org.apache.shiro.subject.Subject;
import org.junit.Test;

import java.util.Arrays;

public class IniRealmTest {
    @Test
    public void IniRealm(){

        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        IniRealm iniRealm = new IniRealm("classpath:shiro.ini");
        defaultSecurityManager.setRealm(iniRealm);

        SecurityUtils.setSecurityManager(defaultSecurityManager);
        Subject subject = SecurityUtils.getSubject();

        UsernamePasswordToken token = new UsernamePasswordToken("fun","123456");
        try {
            subject.login(token);
            System.out.println("Authenticator:"+subject.isAuthenticated());
            System.out.println("登陆成功");
        }catch (AuthenticationException e){
            e.printStackTrace();
            System.out.println("认证失败");
        }

        System.out.println("--------角色验证---------");
        System.out.println("是否具备Manger权限："+subject.hasRole("Manager"));
        System.out.println("是否具备Otheruser权限："+subject.hasRole("Otheruser"));
        System.out.println("是否同事具备Manger和Otheruser权限："+subject.hasAllRoles(Arrays.asList("Manager","Otheruser")));

        System.out.println("---------权限------------");
        System.out.println("是否具备user:select权限："+subject.isPermitted("user:select"));
        System.out.println("是否具备user:add权限："+subject.isPermitted("user:add"));
        System.out.println("是否具备user:delete权限："+subject.isPermitted("user:delete"));
        System.out.println("是否具备user:update权限："+subject.isPermitted("user:update"));
    }
}
```
```
Authenticator:true
登陆成功
--------角色验证---------
是否具备Manger权限：false
是否具备Otheruser权限：true
是否同事具备Manger和Otheruser权限：false
---------权限------------
是否具备user:select权限：true
是否具备user:add权限：false
是否具备user:delete权限：false
是否具备user:update权限：false
```
### JdbcRealm
> dbcRelam 顾名思义，即通过通过访问数据库来获取用户，角色，权限信息。
#### 创建数据
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

-- 导出  表 funimg.roles_permissions 结构
CREATE TABLE IF NOT EXISTS `roles_permissions` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `role_name` varchar(100) DEFAULT NULL,
  `permission` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `idx_roles_permissions` (`role_name`,`permission`)
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8;

-- 正在导出表  funimg.roles_permissions 的数据：~5 rows (大约)
/*!40000 ALTER TABLE `roles_permissions` DISABLE KEYS */;
INSERT INTO `roles_permissions` (`id`, `role_name`, `permission`) VALUES
	(6, 'Manager', 'user:add'),
	(1, 'Manager', 'user:delete'),
	(2, 'Manager', 'user:select'),
	(5, 'Manager', 'user:update'),
	(7, 'Otheruser', 'user:select');
/*!40000 ALTER TABLE `roles_permissions` ENABLE KEYS */;

-- 导出  表 funimg.users 结构
CREATE TABLE IF NOT EXISTS `users` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `username` varchar(100) DEFAULT NULL,
  `password` varchar(100) DEFAULT NULL,
  `password_salt` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `idx_users_username` (`username`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;

-- 正在导出表  funimg.users 的数据：~2 rows (大约)
/*!40000 ALTER TABLE `users` DISABLE KEYS */;
INSERT INTO `users` (`id`, `username`, `password`, `password_salt`) VALUES
	(1, 'admin', '123456', NULL),
	(2, 'fun', '123456', NULL);
/*!40000 ALTER TABLE `users` ENABLE KEYS */;

-- 导出  表 funimg.user_roles 结构
CREATE TABLE IF NOT EXISTS `user_roles` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `username` varchar(100) DEFAULT NULL,
  `role_name` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `idx_user_roles` (`username`,`role_name`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;

-- 正在导出表  funimg.user_roles 的数据：~2 rows (大约)
/*!40000 ALTER TABLE `user_roles` DISABLE KEYS */;
INSERT INTO `user_roles` (`id`, `username`, `role_name`) VALUES
	(1, 'admin', 'Manager'),
	(2, 'fun', 'Otheruser');
/*!40000 ALTER TABLE `user_roles` ENABLE KEYS */;

/*!40101 SET SQL_MODE=IFNULL(@OLD_SQL_MODE, '') */;
/*!40014 SET FOREIGN_KEY_CHECKS=IF(@OLD_FOREIGN_KEY_CHECKS IS NULL, 1, @OLD_FOREIGN_KEY_CHECKS) */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
```
### 测试Code
```
package com.imgurl;

import com.alibaba.druid.pool.DruidDataSource;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;
import org.junit.Test;
import org.apache.shiro.realm.jdbc.JdbcRealm;

import java.util.Arrays;

public class TestJdbcRealm {
    DruidDataSource datasource = new DruidDataSource();
    {
        datasource.setUrl("jdbc:mysql://localhost:3306/test");
        datasource.setUsername("root");
        datasource.setPassword("root");
    }

    @Test
    public void JdbcRealmTest(){
//        1.创建JdbcRealm
        JdbcRealm jdbcRealm = new JdbcRealm();
        jdbcRealm.setDataSource(datasource);  //设置数据源
        jdbcRealm.setPermissionsLookupEnabled(true);  //设置可查看用户权限

//        2.创建安全管理器
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        defaultSecurityManager.setRealm(jdbcRealm);

//        3.主题请求
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        Subject subject = SecurityUtils.getSubject();

//        4.用户认证
        UsernamePasswordToken token = new UsernamePasswordToken("admin","123456");
        try {
            subject.login(token);
            System.out.println("登录成功");


            System.out.println("--------角色验证---------");
            System.out.println("是否具备Manger权限："+subject.hasRole("Manager"));
            System.out.println("是否具备Otheruser权限："+subject.hasRole("Otheruser"));
            System.out.println("是否同事具备Manger和Otheruser权限："+subject.hasAllRoles(Arrays.asList("Manager","Otheruser")));

            System.out.println("---------权限------------");
            System.out.println("是否具备user:select权限："+subject.isPermitted("user:select"));
            System.out.println("是否具备user:add权限："+subject.isPermitted("user:add"));
            System.out.println("是否具备user:delete权限："+subject.isPermitted("user:delete"));
            System.out.println("是否具备user:update权限："+subject.isPermitted("user:update"));
        }catch (AuthenticationException e){
            e.printStackTrace();
            System.out.println("登录失败");
        }
    }
}
```
```
登录成功
--------角色验证---------
是否具备Manger权限：true
是否具备Otheruser权限：false
是否同事具备Manger和Otheruser权限：false
---------权限------------
是否具备user:select权限：true
是否具备user:add权限：true
是否具备user:delete权限：true
是否具备user:update权限：true
```
你会发现我们利用数据库也发现有mysql语句啊。因为我们使用了JdbcRealm的内置语句。我们是按照它语句来命名数据表字段的。下面是代码片段
>  JdbcRealm.class  
```
public class JdbcRealm extends AuthorizingRealm {
    protected static final String DEFAULT_AUTHENTICATION_QUERY = "select password from users where username = ?";
    protected static final String DEFAULT_SALTED_AUTHENTICATION_QUERY = "select password, password_salt from users where username = ?";
    protected static final String DEFAULT_USER_ROLES_QUERY = "select role_name from user_roles where username = ?";
    protected static final String DEFAULT_PERMISSIONS_QUERY = "select permission from roles_permissions where role_name = ?";
    private static final Logger log = LoggerFactory.getLogger(JdbcRealm.class);
    protected DataSource dataSource;
    protected String authenticationQuery = "select password from users where username = ?";
    protected String userRolesQuery = "select role_name from user_roles where username = ?";
    protected String permissionsQuery = "select permission from roles_permissions where role_name = ?";
    protected boolean permissionsLookupEnabled = false;
    protected JdbcRealm.SaltStyle saltStyle;
```
### 自定义数据库及语句
#### 创建新表
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

-- 导出  表 shirotest.sys_permissions 结构
CREATE TABLE IF NOT EXISTS `sys_permissions` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `role_name` varchar(50) DEFAULT '0',
  `permission` varchar(50) DEFAULT '0',
  PRIMARY KEY (`id`),
  UNIQUE KEY `permissions` (`role_name`,`permission`)
) ENGINE=InnoDB AUTO_INCREMENT=21 DEFAULT CHARSET=latin1;

-- 正在导出表  shirotest.sys_permissions 的数据：~2 rows (大约)
/*!40000 ALTER TABLE `sys_permissions` DISABLE KEYS */;
INSERT INTO `sys_permissions` (`id`, `role_name`, `permission`) VALUES
	(1, 'root', 'user:add'),
	(2, 'root', 'user:delete'),
	(4, 'root', 'user:select'),
	(3, 'root', 'user:update'),
	(5, 'user', 'user:select');
/*!40000 ALTER TABLE `sys_permissions` ENABLE KEYS */;

-- 导出  表 shirotest.sys_roles 结构
CREATE TABLE IF NOT EXISTS `sys_roles` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `username` varchar(50) NOT NULL DEFAULT '0' COMMENT '用户名',
  `role_name` varchar(50) NOT NULL DEFAULT '0' COMMENT '角色名称',
  PRIMARY KEY (`id`),
  UNIQUE KEY `username_rolename` (`username`,`role_name`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=latin1;

-- 正在导出表  shirotest.sys_roles 的数据：~2 rows (大约)
/*!40000 ALTER TABLE `sys_roles` DISABLE KEYS */;
INSERT INTO `sys_roles` (`id`, `username`, `role_name`) VALUES
	(1, 'admin', 'root'),
	(2, 'fun', 'user');
/*!40000 ALTER TABLE `sys_roles` ENABLE KEYS */;

-- 导出  表 shirotest.sys_users 结构
CREATE TABLE IF NOT EXISTS `sys_users` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '用户ID',
  `username` varchar(50) NOT NULL DEFAULT '0' COMMENT '用户名',
  `password` varchar(50) NOT NULL DEFAULT '0' COMMENT '用户密码',
  `salt` varchar(100) NOT NULL DEFAULT '0' COMMENT '盐值',
  `last_ip` varchar(100) NOT NULL DEFAULT '0' COMMENT '最后登录IP',
  PRIMARY KEY (`id`),
  UNIQUE KEY `user_username` (`username`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=latin1;

-- 正在导出表  shirotest.sys_users 的数据：~2 rows (大约)
/*!40000 ALTER TABLE `sys_users` DISABLE KEYS */;
INSERT INTO `sys_users` (`id`, `username`, `password`, `salt`, `last_ip`) VALUES
	(1, 'admin', '123456', '0', '0'),
	(2, 'fun', '123456', '0', '0');
/*!40000 ALTER TABLE `sys_users` ENABLE KEYS */;

/*!40101 SET SQL_MODE=IFNULL(@OLD_SQL_MODE, '') */;
/*!40014 SET FOREIGN_KEY_CHECKS=IF(@OLD_FOREIGN_KEY_CHECKS IS NULL, 1, @OLD_FOREIGN_KEY_CHECKS) */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;

```
其实数据库都差不多，只是没有按照内置的命名。
### 测试Code
```
package com.imgurl;

import com.alibaba.druid.pool.DruidDataSource;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.realm.jdbc.JdbcRealm;
import org.apache.shiro.subject.Subject;
import org.junit.Test;

import java.util.Arrays;

public class CustomizeJdbcRealm {
    DruidDataSource datasource = new DruidDataSource();
    {
        datasource.setUrl("jdbc:mysql://localhost:3306/shirotest");
        datasource.setUsername("root");
        datasource.setPassword("root");
    }


    @Test
    public void CustomizeJdbcRealmTest(){
//        1.创建JdbcRealm
        JdbcRealm jdbcRealm = new JdbcRealm();
        jdbcRealm.setDataSource(datasource);
        jdbcRealm.setPermissionsLookupEnabled(true);

        String sql = "select password from sys_users where username = ?";
        jdbcRealm.setAuthenticationQuery(sql);

        String rolesql = "select role_name from sys_roles where username = ?";
        jdbcRealm.setUserRolesQuery(rolesql);

        String permissionsql = "select permission from sys_permissions where role_name = ?";
        jdbcRealm.setPermissionsQuery(permissionsql);


//        2.创建安全管理器
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        defaultSecurityManager.setRealm(jdbcRealm);

//        2.主体请求
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        Subject subject = SecurityUtils.getSubject();

//        3.认证
        UsernamePasswordToken token = new UsernamePasswordToken("fun","123456");
        try {
            subject.login(token);

            System.out.println("认证是否成功："+subject.isAuthenticated());

            System.out.println("--------角色验证---------");
            System.out.println("是否具备root权限："+subject.hasRole("root"));
            System.out.println("是否具备user权限："+subject.hasRole("user"));
            System.out.println("是否同事具备root和user权限："+subject.hasAllRoles(Arrays.asList("root","user")));
            System.out.println("---------权限------------");
            System.out.println("是否具备user:select权限："+subject.isPermitted("user:select"));
            System.out.println("是否具备user:add权限："+subject.isPermitted("user:add"));
            System.out.println("是否具备user:delete权限："+subject.isPermitted("user:delete"));
            System.out.println("是否具备user:update权限："+subject.isPermitted("user:update"));
        }catch (AuthenticationException e) {
            e.printStackTrace();
            System.out.println("认证失败");
        }
    }
}
```
```
认证是否成功：true
--------角色验证---------
是否具备root权限：false
是否具备user权限：true
是否同事具备root和user权限：false
---------权限------------
是否具备user:select权限：true
是否具备user:add权限：false
是否具备user:delete权限：false
是否具备user:update权限：false
```