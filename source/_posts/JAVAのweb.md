---
title: JAVAのweb.xml
date: 2019-04-26 15:24:00
categories: Java
tags:
  - springboot
---
# JAVAのweb.xml
## 前言
> 要问我web.xml是什么，我也模糊不清。这个大概就是整个项目是一辆车，而web.xml就是这把车钥匙🔑吧，如果没有它或者损坏了它？我们就没有把这个web项目跑起来。所以Web项目中,Web.xml还是很重要的。

## 1.分解
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

  <filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>

  <filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

  <servlet>
    <servlet-name>SpringMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring/spring-*.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
    <async-supported>true</async-supported>
  </servlet>

  <servlet-mapping>
    <servlet-name>SpringMVC</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>


</web-app>

```
### 1.1web-app
>  web-app 是描述文件，里面有的元素是我们对这个项目的定义描述。2.4版本以后无需安装DTD指定元素顺序来写文件。
### 1.2filter
>  Filter对用户请求进行预处理，接着将请求交给Servlet进行处理并生成响应，最后Filter再对服务器响应进行后处理。

### 1.3 servlet
>  Servlet通常称为服务端小程序，是服务端的程序，用于处理及响应客户的请求。

### 1.4 servlet-mapping
>  url-pattern 指的是这个目录下的所以资源都将会到servlet下处理。如果我们修改为*.do则表示复合这样的请求格式都将会被servlet处理。
## 学习资料列表
- [JavaWeb中filter的详解及应用案例](http://www.cnblogs.com/vanl/p/5742501.html)
- [web.xml详解](https://www.cnblogs.com/vanl/p/5737656.html)