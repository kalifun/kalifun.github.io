---
title : GoLang_Day3
date: 2018-09-12 17:28:12
categories: Golang
tags:
  - go
  - golang
---
# Golang_Day3
## 结构体
```
一个结构体（`struct`）就是一个字段的集合。
（而 type 的含义跟其字面意思相符。）
```
```go
package main

import "fmt"

type test struct {
	x int
	y int
}

func main()  {
	fmt.Println(test{2,3})
}
```
```
D:\go_sty\day3>go run struct.go
{2 3}
```
## 结构体字段
```
结构体字段利用点来访问
```
```go
package main

import "fmt"

type test1 struct {
	x int
	y int
}

func main()  {
	v := test1{2,3}
	v.x =  10
	fmt.Println(v.x)
}
```
```
D:\go_sty\day3>go run struct_fields.go
10
```
## 结构体指针
```
结构体字段可以通过结构体指针来访问。
通过指针间接访问是透明的。
```
```go
package main

import "fmt"

type test2 struct {
	x int
	y int
}

func main()  {
	v := test2{2,3}
	p := &v
	p.x = 9
	fmt.Println(v)
}
```
```
D:\go_sty\day3>go run struct_pointers.go
{9 3}
```
## 结构体文法 
```
结构体文法表示通过结构体字段的值作为列表来新分配一个结构体。
使用 Name: 语法可以仅列出部分字段。（字段名的顺序无关。）
特殊的前缀 & 返回一个指向结构体的指针
```
```go
package main

import "fmt"

type test3 struct {
	x,y int
}

var (
	v1 = test3{1,2}
	v2 = test3{x:3}
	v3 = test3{y:4}
	p = &test3{5,6}
)
func main()  {
	fmt.Println(v1,v2,v3,p)
}
```
```
D:\go_sty\day3>go run struct_literals.go
{1 2} {3 0} {0 4} &{5 6}
```
## 数组
```
类型[n]t,有n个类型为t的值数组。
```
```
var a [10]int
定义变量a是有十个整数的数组
数组的长度是其类型的一部分，因此数组不能改变大小。
```
```go
package main

import "fmt"

func main()  {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0],a[1])
	fmt.Println(a)
}
```
```
D:\go_sty\day3>go run array.go
Hello World
[Hello World]
```
## slice
```
一个 slice 会指向一个序列的值，并且包含了长度信息。
[]T 是一个元素类型为 T 的 slice。
```
```go
package main

import "fmt"

func main()  {
	p := []int{2,3,4,5,6,7,8,9}
	fmt.Println("p ==",p)
	for i := 0;i < len(p) ;i++  {
		fmt.Printf("p[%d]==%d\n",i,p[i])
	}
}
```
```
D:\go_sty\day3>go run slice.go
p == [2 3 4 5 6 7 8 9]
p[0]==2
p[1]==3
p[2]==4
p[3]==5
p[4]==6
p[5]==7
p[6]==8
p[7]==9
```
## slice切片
```
silce 可以重新切片，创建一个新的slice值指向相同数组。
s[1:5]
表示从1到4（5-1）的slice元素，含两端。
而s[1:1]则为空，s[1:1+1]则为新元素。
```
```go
package main

import "fmt"

func main()  {
	s := []int{2,423,22,6,8,2,56,9,29}
	fmt.Println("s==",s)
	fmt.Println("s[1:4]==",s[1:4])
	//省略下标表示从0开始
	fmt.Println("s[:5]==",s[:5])
	//省略上标表示到len(s）结束
	fmt.Println("s[2:]",s[2:])
}
```
```
D:\go_sty\day3>go run slicing_slices.go
s== [2 423 22 6 8 2 56 9 29]
s[1:4]== [423 22 6]
s[:5]== [2 423 22 6 8]
s[2:] [22 6 8 2 56 9 29]
```
## 构造slice
```
slice 由函数 make 创建。这会分配一个零长度的数组并且返回一个 slice 指向这个数组：
a := make([]int, 5)  // len(a)=5
为了指定容量，可传递第三个参数到 `make`：
b := make([]int, 0, 5) // len(b)=0, cap(b)=5
b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```
```go
package main

import "fmt"

func main()  {
	a := make([]int,5)
	printSlice("a",a)
	b := make([]int,0,5)
	printSlice("b",b)
	c := b[:2]
	printSlice("c",c)
	d := b[2:5]
	printSlice("d",d)
}
func printSlice(s string,x []int)  {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}
```
```
D:\go_sty\day3>go run making_slice.go
a len=5 cap=5 [0 0 0 0 0]
b len=0 cap=5 []
c len=2 cap=5 [0 0]
d len=3 cap=3 [0 0 0]
```
## nil slice
```
slice 的零值是nil
一个nil的slice的长度和容量为0
```
```go
package main

import "fmt"

func main()  {
	var z []int
	fmt.Println(z,len(z),cap(z))
	if z == nil {
		fmt.Println("nil!!!")
	}
}
```
```
D:\go_sty\day3>go run nil_slice.go
[] 0 0
nil!!!
```
## 向slice添加元素（append）
```
向 slice 添加元素是一种常见的操作，因此 Go 提供了一个内建函数 `append`。
func append (s []T)
append 的第一个参数 s 是一个类型为 T 的数组，其余类型为 T 的值将会添加到 slice。
append 的结果是一个包含原 slice 所有元素加上新添加的元素的 slice。
如果 s 的底层数组太小，而不能容纳所有值时，会分配一个更大的数组。 返回的 slice 会指向这个新分配的数组。
```
```go
package main

import "fmt"

func main()  {
	var z []int
	printSlice("z",z)

	z = append(z,0)
	printSlice("z",z)

	z = append(z,1)
	printSlice("z",z)

	z = append(z,2,3,4,5)
	printSlice("z",z)
}
func printSlice(s string,x []int)  {
	fmt.Printf("%s len=%d cap=%d %v\n",s,len(x),cap(x),x)
}
```
```
D:\go_sty\day3>go run append.go
z len=0 cap=0 []
z len=1 cap=1 [0]
z len=2 cap=2 [0 1]
z len=6 cap=6 [0 1 2 3 4 5]
```
## range
```
for 循环的 range 格式可以对 slice 或者 map 进行迭代循环。
```
```go
package main

import "fmt"

var a = []int{2,35,6,125,78,9,356}

func main()  {
	for x,y := range a{
		fmt.Printf("2**%d=%d\n",x,y)
	}
}
```
```
D:\go_sty\day3>go run range.go
2**0=2
2**1=35
2**2=6
2**3=125
2**4=78
2**5=9
2**6=356
```
## range(续)
```
可以通过赋值给 _ 来忽略序号和值。
```
```go
package main

import "fmt"

func main() {
	pow := make([]int, 10)
	for i := range pow {
		pow[i] = 1 << uint(i)
	}
	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}
}
```
```
D:\go_sty\day3>go run range_continued.go
1
2
4
8
16
32
64
128
256
512
```
## map
```
map 映射键到值。
map 在使用之前必须用 make 而不是 new 来创建；值为 nil 的 map 是空的，并且不能赋值。
```
```go
package main

import "fmt"

type test4 struct {
	lat,long float64
}

var m map[string]test4

func main()  {
	m = make(map[string]test4)
	m["FUN"] = test4{
		40.54412,-884.4445554,
	}
	fmt.Println(m["FUN"])
}
```
```
D:\go_sty\day3>go run map.go
{40.54412 -884.4445554}
```
## map文法
```
map 的文法跟结构体文法相似，不过必须有键名。
如果顶级的类型只有类型名的话，可以在文法的元素中省略键名。
```
```go
package main

import "fmt"

type test5 struct {
	x,y float64
}

var m  = map[string]test5{
	"kali" : test5{
		123.456,456.789,
	},
	"fun" : test5{
		789.123,456.123,
	},
}
//var m  = map[string]test5{
//	"kali" : {123.456,456.789},
//	"fun" : {789.123,456.123},
//}
func main()  {
	fmt.Println(m)
}
```
```
D:\go_sty\day3>go run map_literals.go
map[kali:{123.456 456.789} fun:{789.123 456.123}]
```
## 修改map
###### 在map m 中插入或者修改一个元素
```
m[key] = elem
```
###### 获取元素：
```
elem =  m[key]
```
###### 删除元素：
```
delete(m,key)
```
###### 通过双赋值检测某个键值存在：
```
elem,  ok = m[key]
```
如果key在m中，则“ok”为“true”。不存在则为“false”，并且elem是map元素类型的零值。
同样，当从map中读取某个不存在的键值时，结果是map元素类型的零值。
```go
package main

import "fmt"

func main()  {
	q := make(map[string]int)

	q["fun"] = 45
	fmt.Println(q["fun"])

	q["fun"] = 41
	fmt.Println(q["fun"])

	delete(q,"fun")
	fmt.Println("the value:",q["fun"])

	v, ok := q["fun"]
	fmt.Println("the value:",v,"really",ok)


}
```
```
D:\go_sty\day3>go run mutating_map.go
45
41
the value: 0
the value: 0 really false
```

