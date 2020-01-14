---
title: JavaのImgUrl
date: 2019-05-05 10:56:00
categories: Java
tags:
  - springboot
---
# JavaのImgUrl
## 简介
简单的一个图床网站。需要注册才能上传文件，管理员可以无限制的上传图片，普通注册用户一天只能5张图片。
## 创建环境
-  编辑器：IDEA
-  数据库：MariaDB
-  后端框架：SSM+Shiro
-  前端框架：Boostrap + Flat UI
-  服务器：Tomcat8.5以上

## 1.项目步骤
### 1.1Maven创建项目
> [IDEA 简单创建SSM框架](https://kalifun.top/archives/184)
### 1.2修改pom.xml
```
    <!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-spring -->
    <dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-spring</artifactId>
      <version>1.4.0</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-ehcache -->
    <dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-ehcache</artifactId>
      <version>1.4.0</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-quartz -->
    <dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-quartz</artifactId>
      <version>1.4.0</version>
    </dependency>


    <!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-web -->
    <dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-web</artifactId>
      <version>1.4.0</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-core -->
    <dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-core</artifactId>
      <version>1.4.0</version>
    </dependency>

    <!--excel-->
    <dependency>
      <groupId>org.apache.poi</groupId>
      <artifactId>poi</artifactId>
      <version>4.1.0</version>
    </dependency>

    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
    <dependency>
      <groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper</artifactId>
      <version>4.1.6</version>
    </dependency>

    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.17</version>
    </dependency>
```
 将项目需要使用的包先导入，后期导入也没关系。但是springmvc，jdbc，shiro需要提前导入，方便修改web.xml文件。
### 1.3修改web.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->

<!--
  - This is the Cocoon web-app configurations file
  -
  - $Id$
  -->
<web-app version="2.4"
         xmlns="http://java.sun.com/xml/ns/j2ee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
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

  <filter>
    <filter-name>shiroFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    <init-param>
      <param-name>targetFilterLifecycle</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>shiroFilter</filter-name>
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
  </servlet>

  <servlet-mapping>
    <servlet-name>SpringMVC</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

</web-app>
```
### 1.4 创建spring-mvc.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd">
    <context:component-scan base-package="com.imgurl.controller" />
    <mvc:annotation-driven />

    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
        <property name="securityManager" ref="securityManager" />
        <property name="loginUrl" value="/login.jsp" />
        <property name="filterChainDefinitions">
            <value>
                /login=anon
                /admin/**=authc
            </value>
        </property>
    </bean>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="WEB-INF/views" />
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
```
### 1.5创建spring-shiro.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:util="http://www.springframework.org/schema/util"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">

    <!-- Shiro的Web过滤器 -->
    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
        <!-- Shiro的安全管理器，所有关于安全的操作都会经过SecurityManager -->
        <property name="securityManager" ref="securityManager"/>
        <!-- 系统认证提交地址，如果用户退出即session丢失就会访问这个页面 -->
        <property name="loginUrl" value="/login.jsp"/>
        <!-- 权限验证失败跳转的页面，需要配合Spring的ExceptionHandler异常处理机制使用 -->
        <property name="unauthorizedUrl" value="/unauthorized.jsp"/>
        <property name="filters">
            <util:map>
                <entry key="authc" value-ref="formAuthenticationFilter"/>
            </util:map>
        </property>
        <!-- 自定义的过滤器链，从上向下执行，一般将`/**`放到最下面 -->
        <property name="filterChainDefinitions">
            <value>
                <!-- 静态资源不拦截 -->
                /static/** = anon
                /lib/** = anon
                /js/** = anon

                <!-- 登录页面不拦截 -->
                /login.jsp = anon
                /login.do = anon

                <!-- Shiro提供了退出登录的配置`logout`，会生成路径为`/logout`的请求地址，访问这个地址即会退出当前账户并清空缓存 -->
                /logout = logout

                <!-- user表示身份通过或通过记住我通过的用户都能访问系统 -->
                /index.jsp = user

                <!-- `/**`表示所有请求，表示访问该地址的用户是身份验证通过或RememberMe登录的都可以 -->
                /** = user
            </value>
        </property>
    </bean>

    <!-- 基于Form表单的身份验证过滤器 -->
    <bean id="formAuthenticationFilter" class="org.apache.shiro.web.filter.authc.FormAuthenticationFilter">
        <property name="usernameParam" value="username"/>
        <property name="passwordParam" value="password"/>
        <property name="loginUrl" value="/login.jsp"/>
    </bean>

    <!-- 安全管理器 -->
    <bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
        <property name="realm" ref="userRealm" />
    </bean>

    <!-- Realm实现 -->
    <bean id="userRealm" class="com.imgurl.realm.UserRealm"></bean>
</beans>
```
### 1.6创建UserRealm实例
```

```
