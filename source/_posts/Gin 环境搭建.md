---
title : Gin 环境搭建
date: 2018-10-24 17:28:12
categories: Golang
tags :
  - golang
  - gin
  - go
---
# Gin 环境搭建
> Gin是用Golang开发的一个微框架，类似Martinier的API，重点是小巧、易用、性能好很多，也因为 [httprouter](https://github.com/julienschmidt/httprouter) 的性能提高了40倍。

## 1.安装Golang  
根据自己环境安装对应的环境。  

## 2.安装Govendor
GOlang包管理工具 

```go
go get -u github.com/kardianos/govendor
govendor -version
有版本号说明安装成功啦。
```
## 3.安装Gin
```go
go get -u github.com/gin-gonic/gin
```
以下测试代码

```go
package main

import (
	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()
	r.GET("/ping",func(c *gin.Context){
		c.JSON(200,gin.H{
			"message":"tong", 
		})
	})
	r.Run() // listen and serve on 0.0.0.0:8080
}
```
```go
go run xxx.go
```
访问127.0.0.1:8080/ping，若返回{"message":"tong"}则正确

```bash
curl  127.0.0.1:8080/ping 
```
```bash
 % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    18  100    18    0     0    391      0 --:--:-- --:--:-- --:--:--   391{"message":"tong"}
```