---
title: Spring Boot从入门到入门
date: 2019-04-29 12:13:00
categories: Java
tags:
  - springboot
---
# Spring Boot从入门到入门
## 什么是Spring Boot
> #### Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。
## Spring Boot的优点
- ### 自动配置：无需繁琐的去配置web.xml，spring-mvc.xml等
## 新建项目
### 1.选择 Spring Initializr 
![![e98e4d34feff6ab4.png](https://image.kalifun.top/upload/1904/e98e4d34feff6ab4.png)](https://image.kalifun.top/upload/1904/e98e4d34feff6ab4.png)
### 2.设置项目信息
![![e3600e2d4e78a533.png](https://image.kalifun.top/upload/1904/e3600e2d4e78a533.png)](https://image.kalifun.top/upload/1904/e3600e2d4e78a533.png)
### 3.选择WEB
![![3b30c3c7c9615614.png](https://image.kalifun.top/upload/1904/3b30c3c7c9615614.png)](https://image.kalifun.top/upload/1904/3b30c3c7c9615614.png)
### 4.确认存放路径
![![41bdcb9c19891132.png](https://image.kalifun.top/upload/1904/41bdcb9c19891132.png)](https://image.kalifun.top/upload/1904/41bdcb9c19891132.png)
### 5.选择自动导入
#### 当我们修改pom.xml时，它会自动导入包，所以建议选择自动导入。
![![56ad9edae31845d5.png](https://image.kalifun.top/upload/1904/56ad9edae31845d5.png)](https://image.kalifun.top/upload/1904/56ad9edae31845d5.png)
### 6.加载完毕
![![f4a64e176802e8f8.png](https://image.kalifun.top/upload/1904/f4a64e176802e8f8.png)](https://image.kalifun.top/upload/1904/f4a64e176802e8f8.png)
- #### DemoApplication 是一个启动程序。你一个直接选择run它。
- #### application.properties 将需要的配置往里塞。前面ssm中，我们存放的是数据库配置。
- #### DemoApplicationTests 一个测试程序，可以利用它快速编写测试程序某个功能实现。
- #### pom.xml 就不需要解释啦。[Maven](https://kalifun.top/archives/110)构建说明文件。

## 后续配合案例