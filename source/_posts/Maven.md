---
title:  Maven
date: 2019-03-13 16:51:00
categories: Java
tags:
  - springboot
---
  # Maven
## 什么是Maven
> ### Maven 是项目构建工具，能把项目抽象成 POM (project object model), Maven 使用 POM对项目进行构建、打包、文档化等操作。最重要的是解决了项目需要类库的依赖管理，简化了项目开发环境搭建的过程。
####  这个就很像我们学习Django中的django-admin相似，但是有缺少库管理。这样应该就更能理解了。
## 安装Maven
#### 下载链接：[Maven](http://maven.apache.org/)
####  阿里云的[Maven镜像](https://maven.aliyun.com/mvn/view)
#### 1.解压到电脑的任意位置。
#### 2.修改settings
``` xml
<localRepository>path</localRepository>   // maven目录下创建一个repository
// 修改maven的源
<mirrors>
    <mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>        
    </mirror>
  </mirrors>
```
#### 3.设置环境变量(将Maven目录下的bin设置到path中)
#### 4.测试是否成功
```bash
$ mvn -version
Apache Maven 3.6.0 (97c98ec64a1fdfee7767ce5ffb20918da4f719f3; 2018-10-25T02:41:47+08:00)
Maven home: D:\xxxxxxx\xxxx
Java version: 1.8.0_201, vendor: Oracle Corporation, runtime: C:\Program Files\Java\jre1.8.0_201
Default locale: zh_CN, platform encoding: GBK
OS name: "xxxxxx", version: "xxx", arch: "xxx", family: "xxxxx"
```
## 学习资料
### [maven(一) maven到底是个啥玩意~](https://www.cnblogs.com/whgk/p/7112560.html)