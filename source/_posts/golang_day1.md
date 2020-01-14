---
title : GoLang_Day1
date: 2018-09-10 17:28:12
categories: Golang
tags:
  - go
  - golang
---
# GoLang
## 创建hello.go
``` go
package main

import "fmt"

func main()  {
	fmt.Println("Hello")
}
```
## 执行hello.go
``` go
go run hello.go
Hello
```
## 编译
``` go
go build hexxo.go
会生成一个hello.exe
执行hello.exe
Hello （结果）
```
## 代码理解
```
如果是为了将代码编译成一个可执行程序，那么package必须是main
如果是为了将代码编译成库，那么package则没有限制
```
- main()为程序的主函数
- fmt 是go的一个系统库 （路径为C:\Go\src\fmt）
- fmt.Println()打印输出

## 定义变量
格式：
```
var 变量名 变量类型
go语言中定义的函数必须被用到，否则会报错
同时定义变量和赋值可以通过： 变量名：=值
```
```go
package main

import "fmt"

//import "fmt"
//var c,python,java bool
//func main()  {
//	var i int
//	fmt.Println(i,c,python,java)
//}

var i,j  = 1,2

func main()  {
	var c,python,java = true,false,"no!"
	fmt.Println(i,j,c,python,java)
}
```
## 函数
```go
package main

import "fmt"

func add(x int,y int)  int {
	return x+y
}
func main()  {
	fmt.Println(add(100,200))
}
```
```go
当两个或者多个连续的函数命名是同一个类型
则除了最后一个类型外，其他省略。
func add(x,y int)  int {
	return x+y
}
```
## 多值返回
```go
package main

import "fmt"

func swap(x,y string) (string,string) {
	return x,y
}
func main()  {
	a,b := swap("hello","word")
	fmt.Println(a,b)
}
```
## 命名返回值
```go
package main

import "fmt"

func split(sum int) (x,y int) {
	x = sum*4
	y = sum-100
	return  x,y
}
func main()  {
	fmt.Println(split(1000))
}
```
## 类型转换
```go
package main

import (
	"fmt"
	"math"
)

func main()  {
	var x,y int  =  3,4
	var f float64  =  math.Sqrt(float64(x*x+y*y))       //math.Sqrt 计算平方根
	var z int = int(f)
  z := int(f)
	fmt.Println(x,y,z)
}
```
## 常量
```
常量的定义与变量相似，只不过使用const关键词。
常量可以是字符，字符串，数字类型等
常量不能使用 ;= 语法定义。
```
## for循环
```
Go 只有一种循环结构-“for”循环
for 也是Go 的“while”
```
```go
package main

import "fmt"

func main()  {
	sum := 0
	for i := 0 ;i < 10 ; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```
## if 
```go
package main

import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x)+ "i"
	}
	return  fmt.Sprint(math.Sqrt(x))
}
func main()  {
	fmt.Println(sqrt(2),sqrt(-4))
}
//func pow(x,n,lim float64) float64  {
//	if v := math.Pow(x,n); v < lim{
//		return v
//	}
//	return lim
//}
//func main()  {
//	fmt.Println(
//		pow(3,2,10),
//		pow(3,3,20),
//		)
//}
值为：1.4142135623730951 2i
```
## if else
```go
package main

import (
	"fmt"
	"math"
)

func pow(x,n,lim float64) float64 {
	if v := math.Pow(x,n); v < lim{
		return v
	}else {
		fmt.Printf("%g>=%g\n",v,lim)
	}
	return lim
}
func main()  {
	fmt.Println(
		pow(3,2,10),
		pow(3,3,20),
		)
}
值为：27>=20
           9 20
```

## 循环和函数
```作为练习函数和循环的简单途径，用牛顿法实现开方函数。
在这个例子中，牛顿法是通过选择一个初始点 z 然后重复这一过程求 Sqrt(x) 的近似值：
z = z - (z*z-x)/(2*z)
为了做到这个，只需要重复计算 10 次，并且观察不同的值（1，2，3，……）是如何逐步逼近结果的。
然后，修改循环条件，使得当值停止改变（或改变非常小）的时候退出循环。观察迭代次数是否变化。
结果与 [[http://golang.org/pkg/math/#Sqrt][math.Sqrt] 接近吗？
提示：定义并初始化一个浮点值，向其提供一个浮点语法或使用转换：

z := float64(1)
z := 1.0
```
```go
package main

import "fmt"

func sqrt(x float64) float64 {
	const a  =  0.000001
	z := float64(1)
	k := float64(0)
	for ;; z = z - (z*z-x)/(2*z) {
		if z-k <= a && z-k >= -a {
			return z
		}
		k=z
	}
}
func main()  {
	fmt.Println(sqrt(2))
}
值为：1.4142135623730951
```
## switch
```
switch 的条件从上到下的执行，当匹配成功的时候停止。
```
```go
package main

import (
	"fmt"
	"runtime"
)

func main()  {
	fmt.Print("go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("os x")
	case "linux":
		fmt.Println("linux")
	default:
		fmt.Printf("%s",os)
	}
}
```
## defer
```
defer语句会延迟函数的执行直到上层函数返回。
```
```go
package main

import "fmt"

func main()  {
	defer fmt.Println("world")
	fmt.Println("hello")
}
值为：hello
           world
```
