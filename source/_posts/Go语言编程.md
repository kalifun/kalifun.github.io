---
title: Go语言编程
date: 2018-10-30 20:01:00
categories: Golang
tags:
  - golang
---
# Go语言编程  
## 1.0数组切片  
### 1.1基于数组
基于数组我们很好理解，先创建数组再创建切片。
```go
package main

import (
	"fmt"
)

func main() {
	var firstarry [10]int = [10]int{1,2,3,4,5,6,7,8}
	// 创建一个数组
	var secendarry []int = firstarry[:5]
	// 创建一个切片数组

	fmt.Println("This is fist arry :")
	for _, v := range firstarry {
		fmt.Print(v," ")
	}

	fmt.Println("\nThis is second arry :")
	for _, v := range secendarry {
		fmt.Print(v," ")
	}
}
```
```bash
$ go run slice.go
This is fist arry :
1 2 3 4 5 6 7 8 0 0
This is second arry :
1 2 3 4 5
```
可以看出和python很相似，可以更具自己需要的位置选取，array[start:finall]。  
### 1.2直接创建  
Go内置函数make()可以直接创建数组切片。
创建初始元素个数为5的数组切片   
```go 
myslice := make([]int,5)
```
 创建一个初始元素个数为5且预留了10个元素的储存空间
```go
myslice := make([]int,5,10)
```
直接创建一个初始化元素为5的数组切片
```go
myslice := []int{1,2,3,4,5}
```
```go
package main

import (
	"fmt"
)

func main() {
	myslice := make([]int,5,10)
	fmt.Println("myslice len:",len(myslice))
	fmt.Println("myslice cap:",cap(myslice))
  	// myslice := []int{1, 2, 3, 4, 5, 6}
	// fmt.Println("myslice len:", len(myslice))
	// fmt.Println("myslice cap:", cap(myslice))
	//myslice := make([]int, 5)
	//fmt.Println("myslice len:", len(myslice))
	//fmt.Println("myslice cap:", cap(myslice))
}
```
```go
$ go run slice1.go
myslice len: 5
myslice cap: 10  

myslice len: 6
myslice cap: 6  

myslice len: 5
myslice cap: 5
```
### 1.3动态增减元素  
我们可以利用append()对数组切片进行新增元素。
```go
package main

import (
	"fmt"
)

func main() {
	myslice := make([]int, 5)
	fmt.Println("myslice len:", len(myslice))
	fmt.Println("myslice cap:", cap(myslice))
	newslice := append(myslice, 1, 2, 3, 4)
	for _, v := range newslice {
		fmt.Print(v, " ")
	}
	fmt.Println("\nnewslice len:", len(newslice))
	fmt.Println("newslice cap:", cap(newslice))

	myslice2 := make([]int,5,10)
	// fmt.Println("myslice2 len:",len(myslice))
	// fmt.Println("myslice2 cap:",cap(myslice))
	myslice3 := []int{1, 2, 3, 4, 5, 6}
	// fmt.Println("myslice3 len:", len(myslice))
	// fmt.Println("myslice3 cap:", cap(myslice))
	myslice4 := append(myslice2,myslice3...)
	fmt.Println("\nmyslice4 len:",len(myslice4))
	fmt.Println("myslice4 cap:",len(myslice4))
	for _, x := range myslice4 {
		fmt.Print(x," ")
	}
}
```
```go 
$ go run append.go
myslice len: 5
myslice cap: 5
0 0 0 0 0 1 2 3 4
newslice len: 9
newslice cap: 10

myslice4 len: 11
myslice4 cap: 11
0 0 0 0 0 1 2 3 4 5 6
```
利用append（）将会在原数组后面新增元素。原数组长度为5，新增了1，2，3，4所以长度变成了9。两个数组append在一起时，一定不能缺少那个省略号，否则会编译失败。

### 1.4基于数组切片创建数组切片
```go
package main

import (
	"fmt"
)

func main() {
	oldslice := []int{1, 2, 3, 4, 5, 6, 7}
	newslice := oldslice[:4]
	for _, v := range newslice {
		fmt.Print(v, " ")
	}
}
```
```go
$ go run slice2.go
1 2 3 4
```
### 内容复制
```go
package main

import (
	"fmt"
)

func main() {
	slice1 := []int{1, 2, 3, 4, 5}
	slice2 := []int{5, 4}
	copy(slice1, slice2)
	fmt.Println("slice1:")
	for _, v := range slice1 {
		fmt.Print(v, " ")
	}
	// copy(slice2, slice1)
	// fmt.Println("\nslice2:")
	// for _, x := range slice2 {
	// 	fmt.Print(x, " ")
	// }
}
```
```go
$ go run copy.go
slice1:
5 4 3 4 5
把copy(slice1,slice2)一下内容注释后
slice2:
1 2
```
##### 我们可以看出，copy不会改变array的大小。
## 2.0 map
```go
package main

import (
	"fmt"
)

type PersonInfo struct {
	ID      string
	Name    string
	Address string
}

func main() {
	var PersonInfoDB map[string]PersonInfo
	PersonInfoDB = make(map[string]PersonInfo)
	PersonInfoDB["1234"] = PersonInfo{"1234", "FUN", "shanghai"}
	PersonInfoDB["12"] = PersonInfo{"12", "kali", "shanghai"}
	person, ok := PersonInfoDB["1234"]
	if ok {
		fmt.Println("Find person", person.ID,person.Name,person.Address)
	} else {
		fmt.Println("Not Found", person.Name)
	}
}
```
```go
$ go run map.go
Find person 1234 FUN shanghai
```
#### 2.1声明变量
```go
var PersonInfoDB map[string]PersonInfo
```
PersonInfoDB为我们声明的map变量名，[string]则为键的类型，PersonInfo则为所以存放值得类型。  
#### 2.2 创建   
 创建map可以使用make函数。下面创建了一个键为string类型，值类型为PersonInfo的map。
```go
PersonInfoDB = make(map[string]PersonInfo)
```
##### 我们还能在创建时定义map初始的存储能力。以下为粗存能力为100的map。
```
PersonInfoDB = make(map[string]PersonInfo,100)
```
 还能创建直接储存值。  
```go
PersonInfoDB = make(map[string]PersonInfo{"1234", "FUN", "shanghai"})
```
#### 2.3赋值
```go
	PersonInfoDB["1234"] = PersonInfo{"1234", "FUN", "shanghai"}
```
#### 2.4元素删除
利用到delete函数
```go
delete(PersonInfoDB,"1234" )
```
#### 2.5元素查找
```go
person, ok := PersonInfoDB["1234"]
if ok {
  处理数据
}
```
## 3.0函数调用
#### 小写字母开头的函数只在本包内可见，大写字母开头的函数才能被其他包使用。  
## 4.0不定参数
### 4.1 不定参数类型
#### 不定参数类型主要以...type出现。
```go
func myfunc(argr ...int) {
  for _, agr := range argr {
    fmt.Println(agr)
  }
}
```
当我们需要调用函数的时候myfunc(1,2,3).当我们不确定函数类型的时候可以使用...type。
### 4.2任意类型的不定参数
当想传递任意类型，可以指定类型为Interface{}
```go
func printf(format string, args ...interface{}) {
  
}
```
```go
package main

import (
	"fmt"
)

func prinf(args ...interface{}) {
	for _, arg := range args {
		switch arg.(type) {
		case int:
			fmt.Println(arg, "is int value.")
		case string:
			fmt.Println(arg, "is string value.")
		case float32:
			fmt.Println(arg, "is float32 value.")
		default:
			fmt.Println(arg, "is unknow value.")
		}
	}
}

func main() {
	var v1 int = 1
	var v2 int64 = 234
	var v3 string = "hello"
	var v4 float32 = 1.234
	prinf(v1, v2, v3, v4)
}
```
```go
$ go run interface.go
1 is int value.
234 is unknow value.
hello is string value.
1.234 is float32 value.
```
## 5.0面向对象
 类型都是基于值传递的。要想修改变量的值，只能传递指针。
```go
package main

import (
	"fmt"
)

type Integer int

func (a Integer) add(b Integer) {
	a += b
}

func main() {
	var a Integer = 1
	a.add(2)
	fmt.Println("a =", a)
}
```
```
$ go run add.go
a = 1
```
```go
package main

import (
	"fmt"
)

type Integer int

func (a *Integer) add(b Integer) {
	*a += b
}

func main() {
	var a Integer = 1
	a.add(2)
		fmt.Println("a = ",a)
}
```
```go
$ go run add1.go
a =  3
```