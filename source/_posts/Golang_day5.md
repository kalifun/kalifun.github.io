---
title : GoLang_Day5
date: 2018-09-16 17:28:12
categories: Golang
tags:
  - go
  - golang
---
# GoLang_Day5
## 1.字符串类型
```
字符串表示很简单，就是由双引号（“ ”），“单”引号（` `）来描述。注意（` `）非（' '）
唯一区别在于，双引号转义字符可以转义，单引号无法转义。
```
```go
package main

import "fmt"


func main()  {
	var a = "hello \n world"
	var b = `hello \n world`
	fmt.Println(a)
	fmt.Println("-------")
	fmt.Println(b)
}
```
```
D:\go_sty\day5>go run string.go
hello
 world
-------
hello \n world
```
## panic && recover
```
panic && recover是GO语言用来处理异常的关键词
panic用来触发异常的，而recover是用来停止异常并返回传递给panic的值。
注：recover不能异常处理，而recover只能在defer里面，否则无效。
```
```go
package main

import "fmt"

func main()  {
	fmt.Println("I am working")
	panic("i will down?")
	msg := recover()
	fmt.Println(msg)
}
```
```
D:\go_sty\day5>go run panic_recover.go
I am working
panic: i will down?

goroutine 1 [running]:
main.main()
        D:/go_sty/day5/panic_recover.go:7 +0x80
exit status 2
```
## 指针回顾
```
& 取一个变量的地址
*   去一个指针变量所指向的地址值
指针的一大用途就是可以将变量的指针作为实参传递给函数，
从而在函数内部能够直接修改实参所指向的变量值。
```
```go
package  main

import "fmt"

func change(x int)  {
	x = 200
}

func main()  {
	var x int = 100
	fmt.Println(x)
	change(x)
	fmt.Println(x)
}
```
```
D:\go_sty\day5>go run point.go
100
100
```
```
change函数改变的仅仅是内部变量x的值，而不会改变传递进去的实参。
其实，也就是说Go的函数一般关心的是输出结果，而输入参数就相当于信使跑到函数门口大叫，
你们这个参数是什么值，那个是什么值，然后就跑了。你函数根本就不能修改它的值。
不过如果是传递的实参是指针变量，那么函数一看，小子这次你地址我都知道了，哪里跑。
那么就是下面的例子：
```
```go
package  main

import "fmt"

func change(x *int)  {
	*x = 200
}

func main()  {
	var x int = 100
	fmt.Println(x)
	change(&x)
	fmt.Println(x)

}
```
```
上面的例子中，change函数的虚参为整型指针变量，所以在main中调用的时候传递的是x的地址。
然后在change里面使用*x=200修改了这个x的地址的值。所以x的值就变了。
```
```
D:\go_sty\day5>go run point_1.go
100
200
```
## new
```
new这个函数挺神奇，因为它的用处太多了。这里还可以通过new来初始化一个指针。上面说过指针指向(存储)的是一个变量的地址，但是指针本身也需要地址存储。
```
```go
package main

import "fmt"

func set_value(x *int)  {
	*x = 200
}
func main()  {
	x := new(int)
	set_value(x)
	fmt.Println(x)
	fmt.Println(&x)
	fmt.Println(*x)
}
```
```
D:\go_sty\day5>go run new.go
0xc00004a058
0xc000072018
200
```
## 接口
```
Go语言提供了一种接口功能，它把所有的具有共性的方法定义在一起，
任何其他类型只要实现了这些方法就是实现了这个接口，
不一定非要显式地声明要去实现哪些接口啦。
```
```go
package main

import "fmt"

type Phone interface {
	call()
}

type NokiaPhone struct {

}

func (nokiaphone NokiaPhone) call() {
	fmt.Println("I am NokiaPhone")

}

type IPhone struct {
	
}

func (iphone IPhone) call() {
	fmt.Println("I am IPhone")
}

func main()  {
	var phone Phone

	phone  =  new(NokiaPhone)
	phone.call()

	phone = new(IPhone)
	phone.call()
}
```
```
D:\go_sty\day5>go run interface.go
I am NokiaPhone
I am IPhone
```
## 并行计算
