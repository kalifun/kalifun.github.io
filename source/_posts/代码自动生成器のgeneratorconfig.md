---
title: 代码自动生成器のgeneratorconfig.xml
date: 2019-05-08 16:41:00
categories: Java
tags:
  - springboot
---
# 代码自动生成器のgeneratorconfig.xml
## 前言
>当我们写Java项目的时候，我们和数据相关时必定要写Dao，DaoMapper，Model相关的实体和接口。我觉得很无脑的敲，虽然很多都有补全，但是感觉还是没有自动生成快。
## 配置pom.xml
```
<dependency>
      <groupId>org.mybatis.generator</groupId>
      <artifactId>mybatis-generator-core</artifactId>
      <version>1.3.5</version>
    </dependency>
    ---------------------------------------------------------
    下面是写在<build>下面的
    <plugin>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-maven-plugin</artifactId>
        <version>1.3.5</version>
        <configuration>
          <verbose>true</verbose>
          <overwrite>true</overwrite>
        </configuration>
      </plugin>
```
## 在recourse下面创建application.properties
```
db.example.type=mysql
db.example.driver=com.mysql.jdbc.Driver
db.example.url=jdbc:mysql://localhost:3306/bookshop?useUnicode=true&characterEncoding=utf8
db.example.username=username
db.example.password=password

#MBGInfo
generator.location=D:/apache-maven-3.6.0/repository/mysql/mysql-connector-java/5.1.41/mysql-connector-java-5.1.41.jar
generator.targetPackage=com.kalifun.dao   //需要存放自动生成的目录
gererator.schema=database    //数据库名称
gererator.tableName=book     //需要生成的表名称
gererator.objectName=Book  //自动生成的名称
```
## recourse下创建generatorConfig.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- 引入配置文件 -->
    <properties resource="application.properties"/>

    <!-- 指定数据库连接驱动jara地址 -->
    <classPathEntry
            location="${generator.location}" />

    <!-- 一个数据库一个context -->
    <context id="sqlserverTables">
        <!-- 生成的pojo，将implements Serializable -->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin"></plugin>

        <!-- 注释 -->
        <commentGenerator>
            <property name="suppressAllComments" value="true" /><!-- 是否取消注释 -->
            <!-- <property name="suppressDate" value="true" />  是否生成注释代时间戳 -->
        </commentGenerator>

        <!-- 数据库链接URL、用户名、密码 -->
        <jdbcConnection driverClass="${db.example.driver}"
                        connectionURL="${db.example.url}" userId="${db.example.username}"
                        password="${db.example.password}">
        </jdbcConnection>

        <!-- 类型转换 -->
        <javaTypeResolver>
            <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer true，把JDBC DECIMAL
                和 NUMERIC 类型解析为java.math.BigDecimal -->
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!-- 生成model模型，对应的包路径，以及文件存放路径(targetProject)，targetProject可以指定具体的路径,如./src/main/java，
            也可以使用“MAVEN”来自动生成，这样生成的代码会在target/generatord-source目录下 -->
        <javaModelGenerator targetPackage="${generator.targetPackage}"
                            targetProject="./src/main/java">
            <!-- 是否在当前路径下新加一层schema,eg：fase路径com.oop.eksp.user.model， true:com.oop.eksp.user.model.[schemaName] -->
            <property name="enableSubPackages" value="false" />
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <!--对应的mapper.xml文件 -->
        <sqlMapGenerator targetPackage="${generator.targetPackage}"
                         targetProject="./src/main/java">
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>

        <!-- 对应的Mapper接口类文件 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="${generator.targetPackage}" targetProject="./src/main/java">
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>


        <!-- 列出要生成代码的所有表，这里配置的是不生成Example文件 -->
        <!-- schema即为数据库名 tableName为对应的数据库表 domainObjectName是要生成的实体类 enable*ByExample
                   是否生成 example类   -->
        <table tableName="${gererator.tableName}" domainObjectName="${gererator.objectName}"
               schema="${gererator.schema}"
               enableCountByExample="false" enableUpdateByExample="false"
               enableDeleteByExample="false" enableSelectByExample="false"
               selectByExampleQueryId="false">
            <!-- 忽略列，不生成bean 字段
            <ignoreColumn column="FRED" />-->
            <!-- 指定列的java数据类型
            <columnOverride column="LONG_VARCHAR_FIELD" jdbcType="VARCHAR" />  -->
            <!-- 用于指定生成实体类时是否使用实际的列名作为实体类的属性名。false是 Camel Case风格-->
            <property name="useActualColumnNames" value="false" />
        </table>
    </context>
</generatorConfiguration>

```