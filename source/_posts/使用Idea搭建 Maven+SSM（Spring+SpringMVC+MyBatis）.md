---
title: 使用Idea搭建 Maven+SSM（Spring+SpringMVC+MyBatis）
date: 2019-04-01 16:02:00
categories: Java
tags:
  - springboot
---
# 使用Idea搭建 Maven+SSM（Spring+SpringMVC+MyBatis）
## 1.创建项目
创建Maven项目  选择为webapp，等待加载完项目。
## 2.修改pom.xml
 [Maven Repository: Search/Browse/Explore](https://mvnrepository.com/)
选择里面的包贴到pom.xml中，程序会自己下载包
## 3.构造目录
在main目录下创建文件夹
java/com/java/xxxx
修改java属性为resource root
## 4.修改web.xml
```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <!--1.针对spring配置：读取配置文件-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>WEB-INF/spring/applicationContext.xml</param-value>
  </context-param>

  <!--注册servletcontext监听器，并且将applicationcontext对象放到application域中-->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>WEB-INF/spring/applicationContext.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>*.do</url-pattern>
  </servlet-mapping>

  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>utf-8</param-value>
    </init-param>
    <init-param>
      <param-name>forceEncoding</param-name>
      <param-value>/*</param-value>
    </init-param>
  </filter>

  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
</web-app>

```
## 5.创建applicationContext.xml
```xml

```