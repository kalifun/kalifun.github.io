---
title : Golang fmt格式“占位符”
date: 2018-09-10
categories: Golang
tags:
    - go
    - golang
---
# GoLang fmt格式“占位符”
```go
typr human struct{
  name string
}
var  people = human{name:"FUN"}  
```
```
普通占位符
占位符         说明                                          举例               输出
%v           相应值的默认格式                     printf("%v",people)        {FUN}
%+v          打印结构体时，会添加字段             printf（“%+v”,people）     {name:"FUN"}
```
```
字符串与字节切片
占位符                说明                                  举例                           输出
%s           输出字符串表示（string类型或者[]byte） printf（“%s”,[]byte("GO语言")）     GO语言
%q           安全转义                               printf("%q","go语言")                “go语言”
```
