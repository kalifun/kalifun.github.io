---
title : GoLang_Day4
date: 2018-09-16 17:28:12
categories: Golang
tags:
  - go
  - golang
---

# GoLang_Day4
##  函数
```
Go 语言中同时有函数和方法。
一个方法就是一个包含了接受者的函数，
接受者可以是命名类型或者结构体类型的一个值或者是一个指针。
所有给定类型的方法属于该类型的方法集。
```
```go
package main

import (
	"fmt"
	"math"
)

func main()  {
	fun := func(x,y float64) float64 {
		return math.Sqrt(x*x+y*y)    //25的平方根
	}
	fmt.Println(fun(3,4))
}
```
```
D:\go_sty\day4>go run function.go
5
```
## 函数闭包 
```
Go 函数可以是闭包的。闭包是一个函数值，它来自函数体的外部的变量引用。 函数可以对这个引用值进行访问和赋值；换句话说这个函数被“绑定”在这个变量上。
```
```go
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}
func main()  {
	pos,neg := adder(),adder()
	for i :=0;i<10;i++{
		fmt.Println(
			pos(i),
			neg(-2*i),
			)
	}
}
```
```
D:\go_sty\day4>go run function_closer.go
0 0
1 -2
3 -6
6 -12
10 -20
15 -30
21 -42
28 -56
36 -72
45 -90
```
## 运算符
```
<<   左移n位就是乘以2的n次方     1 <<  10     ,结果为2的10次方：1024
>>   右移n位就是除以2的n次方      1024 >> 10     结果为 1024/2的10次方=1
^    异或   两个数个对应的二进制异或     124 ^ 2      1111100 ^ 0000010 = 1111110 结果为126
```