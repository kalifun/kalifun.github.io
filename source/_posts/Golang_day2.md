---
title : GoLang_Day2
date: 2018-09-12 17:28:12
categories: Golang
tags:
  - go
  - golang
---
# Golang_Day2
## defer 栈
```go
package main

import "fmt"

func main()  {
	fmt.Println("FUN")
	for i :=0; i<10 ;i++ {
		defer  fmt.Println(i)
	}
	fmt.Println("done")
}
```
``` 
D:\go_sty\day2>go run defer_multi.go
FUN
done
9
8
7
6
5
4
3
2
1
0
```
## 指针
```
指针保存了变量的内存地址
类型*T 是指向类型T的值得指针。其零值是“nil”
```
```go
package main

import "fmt"

func main()  {
	i,j := 42,27

	p := &i             
	fmt.Println(*p)
	*p = 31
	fmt.Println(i)

	p = &j
	*p = *p/5
	fmt.Println(j)
}
```
