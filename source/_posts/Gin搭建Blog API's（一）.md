---
title : Gin搭建Blog API's（一）
date: 2018-10-24 17:28:12
categories: Golang
tags :
  - golang
  - gin
  - go
---
# Gin搭建Blog API's（一）
按照[Gin搭建Blog API's (一)](https://github.com/EDDYCJY/blog/blob/master/golang/gin/2018-02-16-Gin%E5%AE%9E%E8%B7%B5-%E8%BF%9E%E8%BD%BD%E4%BA%8C-%E6%90%AD%E5%BB%BABlogAPIs-01.md)教程来的。此为原作者。

本教程利用一款读写配置文件的库。[INI](https://ini.unknwon.io),请提前阅读。

## 1.介绍和初始化项目  
创建新的工作区，在src目录创建gin-blog.

```
$GOPATH
├── bin
├── pkg
└── src
    └── gin-blog
```
### 初始化项目目录
```
gin-blog/
├── conf
├── middleware
├── models
├── pkg
├── routers
└── runtime
```
- #### conf：用于存储配置文件
- #### middleware：应用中间件
- #### models：应用数据库模型
- #### pkg：第三方包
- #### routers 路由逻辑处理
- #### runtime 应用运行时数据

### 初始项目数据库
- ### 下载安装数据库（mysql）
- ### 新建blog数据库
[database.png](https://postimg.cc/QHs8bbTY)
[![database.png](https://i.postimg.cc/kXR27YXC/database.png)](https://postimg.cc/QHs8bbTY)
### 创建blog数据库后，创建以下表格：
#### 1.标签表
```mysql
CREATE TABLE `blog_tag` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(100) DEFAULT '' COMMENT '标签名称',
  `created_on` int(10) unsigned DEFAULT '0' COMMENT '创建时间',
  `created_by` varchar(100) DEFAULT '' COMMENT '创建人',
  `modified_on` int(10) unsigned DEFAULT '0' COMMENT '修改时间',
  `modified_by` varchar(100) DEFAULT '' COMMENT '修改人',
  `state` tinyint(3) unsigned DEFAULT '1' COMMENT '状态 0为禁用、1为启用',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='文章标签管理';
```
[database1.png](https://postimg.cc/KKghFkTg)
[![database1.png](https://i.postimg.cc/s2TjY5DK/database1.png)](https://postimg.cc/KKghFkTg)
#### 2.文章表
```mysql
CREATE TABLE `blog_article` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `tag_id` int(10) unsigned DEFAULT '0' COMMENT '标签ID',
  `title` varchar(100) DEFAULT '' COMMENT '文章标题',
  `desc` varchar(255) DEFAULT '' COMMENT '简述',
  `content` text,
  `created_on` int(11) DEFAULT NULL,
  `created_by` varchar(100) DEFAULT '' COMMENT '创建人',
  `modified_on` int(10) unsigned DEFAULT '0' COMMENT '修改时间',
  `modified_by` varchar(255) DEFAULT '' COMMENT '修改人',
  `state` tinyint(3) unsigned DEFAULT '1' COMMENT '状态 0为禁用1为启用',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='文章管理';
```
[database2.png](https://postimg.cc/XXgJtm46)
[![database2.png](https://i.postimg.cc/jqp7nY12/database2.png)](https://postimg.cc/XXgJtm46)
#### 3.认证表
```mysql
CREATE TABLE `blog_auth` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `username` varchar(50) DEFAULT '' COMMENT '账号',
  `password` varchar(50) DEFAULT '' COMMENT '密码',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO `blog`.`blog_auth` (`id`, `username`, `password`) VALUES (null, 'test', 'test123456');
```
[database3.png](https://postimg.cc/6TB6VcRj)
[![database3.png](https://i.postimg.cc/sfhBr02r/database3.png)](https://postimg.cc/6TB6VcRj)  

## 2.编写项目配置包
### 下载依赖包[go-ini/ini](https://github.com/go-ini/ini)
```go
go get -u  github.com/go-ini/ini
```
### 在gin-blog的conf目录下创建app.ini文件
```
#debug or release
RUN_MODE = debug

[app]
PAGE_SIZE = 10
JWT_SECRET = 23347$040412

[server]
HTTP_PORT = 8000
READ_TIMEOUT = 60
WRITE_TIMEOUT = 60

[database]
TYPE = mysql
USER = 数据库账号
PASSWORD = 数据库密码
#127.0.0.1:3306
HOST = 数据库IP:数据库端口号
NAME = blog
TABLE_PREFIX = blog_
```
### 建立配置setting模块，在pkg目录下创建setting目录，并且创建setting.go文件。
```go
package setting

import (
	"time"
	"log"
	"github.com/go-ini/ini"
)

var (
	config *ini.File
	RunModel string

	Httpport int
	ReadTimeOut time.Duration
	WriteTimeOut time.Duration

	Pagesize int
	Jwtsecret string
)

func init() {
	var err error
	config, err = ini.Load("conf/app.ini") 
	if err != nil {
		log.Fatalf("Fail to read file: %v", err)
	}
	LoadBase()
	LoadServer()
	LoadApp()
}

func LoadBase() {
	RunModel = config.Section("").Key("RUN_MODE").MustString("debug")
}

func LoadServer() {
	ser, err := config.GetSection("server")
	if err != nil {
		log.Fatalf("Fail to get selection %v",err)
	}
	RunModel = config.Section("").Key("RUN_MODE").MustString("debug")

	Httpport = ser.Key("HTTP_PORT").MustInt(8000)
	ReadTimeOut = time.Duration(ser.Key("READ_TIMEOUT").MustInt(60)) * time.Second
	WriteTimeOut = time.Duration(ser.Key("WRITE_TIMEOUT").MustInt(60)) * time.Second
}
func LoadApp() {
	App, err := config.GetSection("app")
	if err != nil {
		log.Fatalf("Fail to get selection %v",err)
	}
	Pagesize = App.Key("PAGE_SIZE").MustInt(10)
	Jwtsecret = App.Key("JWT_SECRET").MustString("!@)*#)!@U#@*!@!)")
}
```
#### 目录树
```
└─gin-blog
    │  test.go
    │
    ├─conf
    │      app.ini
    │
    ├─middleware
    ├─models
    ├─pkg
    │      setting.go
    │
    ├─routers
    └─runtime
```
## 编写API错误码包
#### 建立错误码包的e模块，在gin-blog中的pkg目录下创建e目录，在下创建code.go和msg.go文件。
#### 1.code.go
```go
package e

const (
	SUCCESS = 200
	ERROR = 500
	INVALID_PARAMS = 400

	ERROR_EXIST_TAG = 10001
	ERROR_NOT_EXIST_TAG = 10002
	ERROR_NOT_EXIST_ARTICLE = 10003

	ERROR_AUTH_CHECK_TOKEN_FAIL = 20001
	ERROR_AUTH_CHECK_TOKEN_TIMEOUT = 20002
	ERROR_AUTH_TOKEN = 20003
	ERROR_AUTH = 20004
)
```
#### 2.msg.go
```go
package e

var MsgFlags = map[int]string {
	SUCCESS : "ok",
	ERROR : "fail",
	INVALID_PARAMS : "请求参数错误！",
	ERROR_EXIST_TAG : "已存在该标签名称",
	ERROR_NOT_EXIST_TAG : "该标签不存在",
	ERROR_NOT_EXIST_ARTICLE : "该文章不存在",
	ERROR_AUTH_CHECK_TOKEN_FAIL : "Token鉴权失败",
	ERROR_AUTH_CHECK_TOKEN_TIMEOUT : "Token已超时",
	ERROR_AUTH_TOKEN : "Token生成失败",
	ERROR_AUTH : "Token错误",
}
func GetMsg(code int) string {
	msg, ok := MsgFlags[code]
	if ok {
		return msg
	}
	return MsgFlags[ERROR]
}
```
## 编写工具包
#### 拉取依赖包[com](github.com/Unknwon/com)。
```go
go get -u github.com/Unknwon/com
```
### 编写分页页码的获取方法
#### 在gin-blog的pkg目录下，新建util目录。在util目录下创建pagination.go
####  pagination.go
```go
package util

import (
	"github.com/gin-gonic/gin"
	"github.com/Unknwon/com"
	"gin-blog/pkg/setting"
)

func Getpag(c *gin.Context) int {
	result := 0
	page,_ := com.StrTo(c.Query("page")).Int()
	if page > 0 {
		result = (page - 1) * setting.PageSize
	}
	return result
}
```
[error.png](https://postimg.cc/QHT8wVqB)
[![error.png](https://i.postimg.cc/DyCSWXJc/error.png)](https://postimg.cc/QHT8wVqB)
#### 根据可以看出，我们的目录错了。应该在src中创建gin-blog。可以去看看GO的工作区的使用应该可以明白。  
### 编写models init  
#### 拉取[gorm](github.com/jinzhu/gorm)依赖包。[gorm文档](http://gorm.book.jasperxu.com)
```go
go get -u github.com/jinzhu/gorm
```
#### 拉取[mysql](github.com/go-sql-driver/mysql)驱动依赖包。
```go
go get -u github.com/go-sql-driver/mysql
```
#### 在gin-blog的models目录下新建models.go，用于models的初始化使用.
#### models.go
```go
package models

import (
	"fmt"
	"log"

	"github.com/jinzhu/gorm"
	_ "github.com/jinzhu/gorm/dialects/mysql"

	"gin-blog/pkg/setting"
)

var db *gorm.DB

type Model struct {
	ID int `gorm:"primary_key" json:"id"`
	CreatedOn int `json:"created_on"`
	ModifiedOn int `json:"modified_on"`
}

func init() {
	var (
		err error
		dbType, dbName, user, password, host, tablePrefix string
	)
	// ser,err := setting.config.GetSection("database")
	ser, err := setting.config.GetSection("database")
	if err != nil {
		log.Fatalf("Fail get selection %v",err)
	}

	dbType = ser.Key("TYPE").String()
	dbName = ser.Key("NAME").String()
	user = ser.Key("USER").String()
	password = ser.Key("PASSWORD").String()
	host = ser.Key("HOST").String()
	tablePrefix = ser.Key("TABLE_PREFIX").String()

	db, err = gorm.Open(dbType, fmt.Sprintf("%s:%s@tcp(%s)/%s?charset=utf8&parseTime=True&loc=Local", user, password, host, dbName))
	if err != nil {
		log.Println(err)
	}
	gorm.DefaultTableNameHandler = func (db *gorm.DB, defaultTableName string) string {
		return tablePrefix + defaultTableName;
	}
	db.SingularTable(true)
	db.DB().SetMaxIdleConns(10)
	db.DB().SetMaxOpenConns(100)
}
func CloseDB() {
	defer db.Close()
}
```
[error1.png](https://postimg.cc/75S3Ngtm)
[![error1.png](https://i.postimg.cc/g2TgmsVF/error1.png)](https://postimg.cc/75S3Ngtm)
#### 出现问题。和之前的一样，但是目前的目录已经迁移到src/gin-blog。  

## 编写项目启动，路由文件。
### 编写demo
#### 在gin-blog的目录下创建main.go为启动文件。
```go
package main

import (
	"fmt"
	"gin-blog/pkg/setting"
	"net/http"

	"github.com/gin-gonic/gin"
)

func main() {
	router := gin.Default()
	router.GET("/test", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "test",
		})
	})
	s := &http.Server{
		Addr:           fmt.Sprintf(":%d", setting.Httpport),
		Handler:        router,
		ReadTimeout:    setting.ReadTimeOut,
		WriteTimeout:   setting.WriteTimeOut,
		MaxHeaderBytes: 1 << 20,
	}
	s.ListenAndServe()
}
```
- ##### Addr：监听的TCP地址，格式为:8000
- ##### Handler：http句柄，实质为ServeHTTP，用于处理程序响应HTTP请求
- ##### TLSConfig：安全传输层协议（TLS）的配置
- ##### ReadTimeout：允许读取的最大时间
- ##### ReadHeaderTimeout：允许读取请求头的最大时间
- ##### WriteTimeout：允许写入的最大时间
- ##### IdleTimeout：等待的最大时间
- ##### MaxHeaderBytes：请求头的最大字节数
- ##### ConnState：指定一个可选的回调函数，当客户端连接发生变化时调用
- ##### ErrorLog：指定一个可选的日志记录器，用于接收程序的意外行为和底层系统错误；如果未设置或为nil则默认以日志包的标准日志记录器完成（也就是在控制台输出）
```go
go run main.go
curl 127.0.0.1:8000/test
{"message":"test"}
```
### 修改路由规则
#### 在routers目录下创建router.go
```go
package routers

import (
	"gin-blog/pkg/setting"
	"github.com/gin-gonic/gin"
)

func InitRouter() *gin.Engine {
	r := gin.New()
	r.Use(gin.Logger())
	r.Use(gin.Recovery())

	gin.SetMode(setting.RunModel)

	r.GET("/test",func(c *gin.Context){
		c.JSON(200,gin.H{
			"message":"ok",
		})
	})
  return r
}
```
#### 修改main.go
```go
package main

import (
	"fmt"
	"gin-blog/pkg/setting"
	"net/http"
	"gin-blog/routers"

	// "github.com/gin-gonic/gin"
)

func main() {
	router := routers.InitRouter()
	// router := gin.Default()
	// router.GET("/test", func(c *gin.Context) {
	// 	c.JSON(200, gin.H{
	// 		"message": "test",
	// 	})
	// })
	s := &http.Server{
		Addr:           fmt.Sprintf(":%d", setting.Httpport),
		Handler:        router,
		ReadTimeout:    setting.ReadTimeOut,
		WriteTimeout:   setting.WriteTimeOut,
		MaxHeaderBytes: 1 << 20,
	}
	s.ListenAndServe()
}
```
```go
go run main.go
curl 127.0.0.1:8000/test
{"message":"ok"}
```
