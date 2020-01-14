---
title: IDEA 简单创建SSM框架
date: 2019-04-08 21:02:00
categories: Java
tags:
  - springboot
---
# IDEA 简单创建SSM框架
## SSM框架
>  S:Spring框架  S:Spring-MVC  M:Mybatis
### Spring框架
Spring 是一个轻量级的 DI / IoC 和 AOP 容器的开源框架。
![![](https://image.kalifun.top/upload/1904/b1df24e660b0706a.jpeg)](https://image.kalifun.top/upload/1904/b1df24e660b0706a.jpeg)
### Spring-MVC
 你可以理解Spring-MVC是Spring的一个模块。而MVC则是一种处理模式。现在很多框架都采用这种模式。
 M：model是数据交互层。V：views是视图层。C：Controller是业务逻辑层。
![![](https://image.kalifun.top/upload/1904/2da1900edb713e3b.jpeg)](https://image.kalifun.top/upload/1904/2da1900edb713e3b.jpeg)
### Mybatis
 在我们传统的 JDBC 中，我们除了需要自己提供 SQL 外，还必须操作 Connection、Statment、ResultSet，不仅如此，为了访问不同的表，不同字段的数据，我们需要些很多雷同模板化的代码，闲的繁琐又枯燥。

## 1.Maven创建项目
 注意：
- 已经安装jdk,maven(使用IDEA自带应该也是可以的)
### 1.1创建步骤
![![](https://image.kalifun.top/upload/1904/23ef84aa9eba8d01.png)](https://image.kalifun.top/upload/1904/23ef84aa9eba8d01.png)
 建议选择1吧。
![![](https://image.kalifun.top/upload/1904/7613f966a0034e59.png)](https://image.kalifun.top/upload/1904/7613f966a0034e59.png)
![![7ed744f109877b82.png](https://image.kalifun.top/upload/1904/7ed744f109877b82.png)](https://image.kalifun.top/upload/1904/7ed744f109877b82.png)
![![e71140fd484f9479.png](https://image.kalifun.top/upload/1904/e71140fd484f9479.png)](https://image.kalifun.top/upload/1904/e71140fd484f9479.png)
![![e1f7c4a6f0d70052.png](https://image.kalifun.top/upload/1904/e1f7c4a6f0d70052.png)](https://image.kalifun.top/upload/1904/e1f7c4a6f0d70052.png)
 当出现了上面的字眼，就说明已经创建成功了。
## 2.构造项目
### 2.1项目架构
 先把项目按照图的架构来作准备。
![![879a384754458f38.png](https://image.kalifun.top/upload/1904/879a384754458f38.png)](https://image.kalifun.top/upload/1904/879a384754458f38.png)
### 2.2修改pom.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one
  or more contributor license agreements.  See the NOTICE file
  distributed with this work for additional information
  regarding copyright ownership.  The ASF licenses this file
  to you under the Apache License, Version 2.0 (the
  "License"); you may not use this file except in compliance
  with the License.  You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing,
  software distributed under the License is distributed on an
  "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
  KIND, either express or implied.  See the License for the
  specific language governing permissions and limitations
  under the License.
-->
<!-- $Id: pom.xml 642118 2008-03-28 08:04:16Z reinhard $ -->
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

  <modelVersion>4.0.0</modelVersion>
  <packaging>war</packaging>

  <name>ssmdemo</name>
  <groupId>ssmdemo</groupId>
  <artifactId>ssmdemo</artifactId>
  <version>1.0-SNAPSHOT</version>

  <build>
    <plugins>
      <plugin>
        <groupId>org.mortbay.jetty</groupId>
        <artifactId>maven-jetty-plugin</artifactId>
        <version>6.1.7</version>
        <configuration>
          <connectors>
            <connector implementation="org.mortbay.jetty.nio.SelectChannelConnector">
              <port>8888</port>
              <maxIdleTime>30000</maxIdleTime>
            </connector>
          </connectors>
          <webAppSourceDirectory>${project.build.directory}/${pom.artifactId}-${pom.version}</webAppSourceDirectory>
          <contextPath>/</contextPath>
        </configuration>
      </plugin>
    </plugins>
  </build>

  <properties>
    <!-- 设置项目编码编码 -->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <!-- spring版本号 -->
    <spring.version>4.3.5.RELEASE</spring.version>
    <!-- mybatis版本号 -->
    <mybatis.version>3.4.1</mybatis.version>
  </properties>

  <dependencies>
    <!--dependency>
      <groupId>ssmdemo</groupId>
      <artifactId>[the artifact id of the block to be mounted]</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency-->

    <!-- java ee -->
    <dependency>
      <groupId>javax</groupId>
      <artifactId>javaee-api</artifactId>
      <version>7.0</version>
    </dependency>

    <!-- 单元测试 -->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
    </dependency>

    <!-- 实现slf4j接口并整合 -->
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.2</version>
    </dependency>

    <!-- JSON -->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.8.7</version>
    </dependency>


    <!-- 数据库 -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.41</version>
      <scope>runtime</scope>
    </dependency>

    <!-- 数据库连接池 -->
    <dependency>
      <groupId>com.mchange</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.5.2</version>
    </dependency>

    <!-- MyBatis -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>${mybatis.version}</version>
    </dependency>

    <!-- mybatis/spring整合包 -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.1</version>
    </dependency>

    <!-- Spring -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>${spring.version}</version>
    </dependency>


  </dependencies>

</project>
```
 修改完成后将会自动下载jar包。
### 2.3修改web.xml
配置 DispatcherServlet ，告诉程序去哪里寻找对应的xml文件。
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <!-- 编码过滤器 -->
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

    <!-- 配置DispatcherServlet -->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 配置springMVC需要加载的配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-*.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
        <async-supported>true</async-supported>
    </servlet>
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <!-- 匹配所有请求 -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```
### 2.4创建jdbc.properties
 在resources目录下创建jdbc.properties
```
jdbc.driver=com.mysql.jdbc.Driver
#数据库地址
jdbc.url=jdbc:mysql://localhost:3306/ssm?useUnicode=true&characterEncoding=utf8
#用户名
jdbc.username=root
#密码
jdbc.password=root
#最大连接数
c3p0.maxPoolSize=30
#最小连接数
c3p0.minPoolSize=10
#关闭连接后不自动commit
c3p0.autoCommitOnClose=false
#获取连接超时时间
c3p0.checkoutTimeout=10000
#当获取连接失败重试次数
c3p0.acquireRetryAttempts=2
```
### 2.5创建spring-mybatis.xml
 在resource目录下创建spring-mybatis.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!-- 扫描service包下所有使用注解的类型 -->
    <context:component-scan base-package="com.ssmdeom.service"/>

    <!-- 配置数据库相关参数properties的属性：${url} -->
    <context:property-placeholder location="classpath:jdbc.properties"/>

    <!-- 数据库连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="maxPoolSize" value="${c3p0.maxPoolSize}"/>
        <property name="minPoolSize" value="${c3p0.minPoolSize}"/>
        <property name="autoCommitOnClose" value="${c3p0.autoCommitOnClose}"/>
        <property name="checkoutTimeout" value="${c3p0.checkoutTimeout}"/>
        <property name="acquireRetryAttempts" value="${c3p0.acquireRetryAttempts}"/>
    </bean>

    <!-- 配置SqlSessionFactory对象 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 注入数据库连接池 -->
        <property name="dataSource" ref="dataSource"/>
        <!-- 扫描entity包 使用别名 -->
        <property name="typeAliasesPackage" value="com.ssmdemo.entity"/>
        <!-- 扫描sql配置文件:mapper需要的xml文件 -->
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
    </bean>

    <!-- 配置扫描Dao接口包，动态实现Dao接口，注入到spring容器中 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 注入sqlSessionFactory -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!-- 给出需要扫描Dao接口包 -->
        <property name="basePackage" value="com.ssmdeom.dao"/>
    </bean>

    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- 注入数据库连接池 -->
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- 配置基于注解的声明式事务 -->
    <tx:annotation-driven transaction-manager="transactionManager"/>

</beans>
```
### 2.6创建spring-mvc.xml
 在resource目录下创建spring-mvc.xml
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
       http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">

    <!-- 扫描web相关的bean -->
    <context:component-scan base-package="com.ssmdeom.controller"/>

    <!-- 开启SpringMVC注解模式 -->
    <mvc:annotation-driven/>

    <!-- 静态资源默认servlet配置 -->
    <mvc:default-servlet-handler/>

    <!-- 配置jsp 显示ViewResolver -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="/WEB-INF/views/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```
### 2.7配置日志相关文件
在resource目录下创建logback.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>
    <root level="debug">
        <appender-ref ref="STDOUT"/>
    </root>
</configuration>
```
## 3.创建数据库
```
CREATE TABLE `student` (
  `ID` int(11) NOT NULL AUTO_INCREMENT,
  `stuName` varchar(32) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_520_nopad_ci DEFAULT NULL,
  `stuAge` tinyint(4) DEFAULT NULL,
  `mobile` varchar(11) CHARACTER SET latin1 DEFAULT NULL,
  `address` varchar(256) COLLATE utf8mb4_unicode_520_ci DEFAULT NULL,
  `EntranceTime` date DEFAULT NULL,
  PRIMARY KEY (`ID`)
) ENGINE=InnoDB AUTO_INCREMENT=20 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_520_ci
```
## 4.编写程序
### 4.1在entity下创建Student实体
```
package com.ssmdeom.entity;

import java.util.Date;

public class Student {
    private int id;
    private String stuName;
    private int stuAge;
    private String mobile;
    private String address;
    private Date EntranceTime;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getStuName() {
        return stuName;
    }

    public void setStuName(String stuName) {
        this.stuName = stuName;
    }

    public int getStuAge() {
        return stuAge;
    }

    public void setStuAge(int stuAge) {
        this.stuAge = stuAge;
    }

    public String getMobile() {
        return mobile;
    }

    public void setMobile(String mobile) {
        this.mobile = mobile;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Date getEntranceTime() {
        return EntranceTime;
    }

    public void setEntranceTime(Date entranceTime) {
        EntranceTime = entranceTime;
    }
}
```
### 4.2在Dao下面创建Dao接口
```
package com.ssmdeom.dao;

import com.ssmdeom.entity.Student;

import java.util.List;

public interface StudentDao {

    Student SearchStudentId(int id);
    List<Student> listStudent();

}
```
### 4.3在resources/mapper下编写StudentDao.xml映射文件
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 设置为IUserDao接口方法提供sql语句配置 -->
<mapper namespace="com.ssmdeom.dao.StudentDao">

    <select id="searchStudentId" resultMap="student" parameterType="int">
        SELECT * FROM student WHERE id = #{id}
    </select>

    <resultMap id="student" type="student">
        <id column="id" property="id" />
        <result column="stuName" property="stuName" />
        <result column="stuAge" property="stuAge" />
        <result column="mobile" property="mobile" />
        <result column="address" property="address" />
        <result column="EntranceTime" property="EntranceTime" />
    </resultMap>

    <select id="listStudent" resultMap="student">
        SELECT * FROM student
    </select>
</mapper>
```
### 4.4在service下创建StudentService的接口
```
package com.ssmdeom.service;

import com.ssmdeom.entity.Student;

import java.util.List;

public interface StudentService {

    Student searchStudentId(int id);
    List<Student> listStudent();
}
```
### 4.5在service下创建StudentServiceImpl类
```
package com.ssmdeom.service;


import com.ssmdeom.dao.StudentDao;
import com.ssmdeom.entity.Student;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class StudentServiceImpl implements StudentService {

    @Autowired
    StudentDao studentDao;

    public Student searchStudentId(int id) {
        return studentDao.searchStudentId(id);
    }

    public List<Student> listStudent() {
        return studentDao.listStudent();
    }
}
```
### 4.6在controller下面创建StudentController
```
package com.ssmdeom.controller;


import com.ssmdeom.entity.Student;
import com.ssmdeom.service.StudentService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.List;

@Controller
@RequestMapping("")
public class StudentController {

    @Autowired
    private StudentService studentService;

    @RequestMapping("/all")
    public String listStudent(HttpServletRequest request, HttpServletResponse response) {
        List<Student> students = studentService.listStudent();
        request.setAttribute("students",students);
        return "liststudents";
    }

    @RequestMapping("/student")
    public ModelAndView getstudent(int id) {
        ModelAndView mav = new ModelAndView("student");
        Student studentid = studentService.searchStudentId(id);
        mav.addObject("student",studentid);
        return  mav;
    }
}
```