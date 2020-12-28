---
title: Golang反射(reflect)
date: 2020-01-17 00:00:00
categories: Golang
tags: 
  - golang
---
# Golang反射(reflect)

大表哥我创建了一个数据库，我想通过一个函数就能实现所有表的插入。那你就用传入不定参数进行数据添加啊。可是我觉得好麻烦哦，我还得分析它的类型符不符合，它属于哪个表的数据。这样太费劲了，我还不如直接使用别人的[`orm`](https://baike.baidu.com/item/%E5%AF%B9%E8%B1%A1%E5%85%B3%E7%B3%BB%E6%98%A0%E5%B0%84/311152?fromtitle=ORM&fromid=3583252&fr=aladdin)，都不需要思考就完成了我想要的结果。

我们通过学习`orm`工作流来学习反射，这样又能熟悉使用场景，又能理解含义。

## 什么是反射?

反射就是程序能够帮助我们得知变量的类型和值，让我们可以针对不同类型执行不同函数。也许说到这里还是不太清楚反射到底是什么，那就跟着我往下走，也许你会对反射更加了解。

## 数据操作

我想插入数据到我的表中，你会怎么做？我想很多人立马帕拉帕拉写成像下面例子：

```go
package main

import "fmt"

// 假设我们有一个用户表
type User struct {
	UserName string
	Password string
}

func CreateQuery(u User) string {
	insert := fmt.Sprintf("insert into user values(%v %v)", u.UserName, u.Password)
	return insert
}

func main() {
	u := User{
		UserName: "张三",
		Password: "admin",
	}

	fmt.Println(CreateQuery(u))
}


```

我们会获得结果：

```bash
insert into user values(张三 admin)
```

看样子没毛病啊，嗯~那我们有10个表，你帮我写10个函数可好？才10个函数，没问题啊。那我们表越来越多，我们每次要针对这个表再写一个函数？呜呜，我不干了。那我们想要让一个函数通用的话，我们需要知道哪些东西呢？

没错，在这里就要引用到反射这个东西，我们需要搞清楚我们传递变量的类型和值，根据具体情况再作操作。

## 引用反射

> [`reflect`](https://golang.org/pkg/reflect/) 实现了运行时反射。`reflect` 包会帮助识别 [`interface{}`](https://studygolang.com/articles/12266) 变量的底层具体类型和具体值。

所以我们函数要接收的参数类型不再是自定义类型，而是接收`interface{}`参数，我再根据它的类型和值创建我们需要插入的`SQL`语句。

### reflect.TypeOf与reflect.ValueOf

> 我们通过Type和Value就能知道他们的具体作用就是获取类型和值的。那我们通过具体实例了解它是如何使用的。

```go
package main

import (
	"fmt"
	"reflect"
)

// 假设我们有一个用户表
type User struct {
	UserName string
	Password string
}

func CreateQuery(i interface{}) {
	t := reflect.TypeOf(i)
	v := reflect.ValueOf(i)
	fmt.Println(t)
	fmt.Println(v)
}

func main() {
	u := User{
		UserName: "张三",
		Password: "admin",
	}
	CreateQuery(u)
}
```

我们会获得结果：

```bash
main.User
{张三 admin}
```

### reflect.kind

都说`kind`和`TypeOf`很相似，我们通过实例看看他们有什么共性吧！

```go
package main

import (
	"fmt"
	"reflect"
)

// 假设我们有一个用户表
type User struct {
	UserName string
	Password string
}

func CreateQuery(i interface{}) {
	t := reflect.TypeOf(i)
	k := t.Kind()
	fmt.Println(t)
	fmt.Println(k)
}

func main() {
	u := User{
		UserName: "张三",
		Password: "admin",
	}
	CreateQuery(u)
}

```

我们会获得结果：

```bash
main.User
struct
```

`TypeOf` 表示 `interface{}` 的实际类型（在这里是 **main.User**),`Kind` 表示该类型的特定类别（在这里是 **struct**）

### NumField() 和 Field() 方法

> [`NumField()`](https://golang.org/pkg/reflect/#Value.NumField) 方法返回结构体中字段的数量，而 [`Field(i int)`](https://golang.org/pkg/reflect/#Value.Field) 方法返回字段 `i` 的 `reflect.Value`。

下面我们使用实例看这两个方法的具体使用方式：

```go
package main

import (
	"fmt"
	"reflect"
)

// 假设我们有一个用户表
type User struct {
	UserName string
	Password string
}

func CreateQuery(i interface{}) {
	// 判断传入的类型是否为struct
	if reflect.TypeOf(i).Kind() == reflect.Struct {
		v := reflect.ValueOf(i)
		fmt.Println("结构体字段数量为:", v.NumField())
		for k := 0; k < v.NumField(); k++ {
			fmt.Printf("字段:%d 类型:%T 值:%v\n", k, v.Field(k), v.Field(k))
		}
	}
}

func main() {
	u := User{
		UserName: "张三",
		Password: "admin",
	}
	CreateQuery(u)
}
```

我们会获得结果：

```bash
结构体字段数量为: 2
字段:0 类型:reflect.Value 值:张三
字段:1 类型:reflect.Value 值:admin
```

### 类型转换

当我们获取的Value值和数据库指定类型不匹配时，是无法正常插入这条数据的。所以在`reflect`给我们提供了这个方法，让我们获取的值转换成我们指定的类型。

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	id := 1
	newid := reflect.ValueOf(id).Int()
	name := "kali"
	newname := reflect.ValueOf(name).String()
	tags := make([]string, 3, 10)
	newtags := reflect.ValueOf(tags).Slice(3, 10)
	fmt.Println(newid)
	fmt.Println(newname)
	fmt.Println(newtags)
}
```

我们会获得结果：

```bash
1
kali
[      ]
```

### 完整实例

我们有多个表是如何实现共用函数的呢？下面我们将使用实例来理解。

```go
package main

import (
	"fmt"
	"reflect"
	"strings"
)

type User struct {
	Id       int
	UserName string
	PassWord string
}

type Permission struct {
	RoleId int
	Leve   int
	Des    string
}

func CreateQuery(inter interface{}) {
	if reflect.TypeOf(inter).Kind() == reflect.Struct {
		t := reflect.TypeOf(inter).Name()
		// 将其全为小写
		table := strings.ToLower(t)
		query := fmt.Sprintf("insert into %s values(", table)
		v := reflect.ValueOf(inter)
		for i := 0; i < v.NumField(); i++ {
			switch v.Field(i).Kind() {
			case reflect.Int:
				if i == 0 {
					query = fmt.Sprintf("%s%d", query, v.Field(i).Int())
				} else {
					query = fmt.Sprintf("%s, %d", query, v.Field(i).Int())
				}
			case reflect.String:
				if i == 0 {
					query = fmt.Sprintf("%s\"%s\"", query, v.Field(i).String())
				} else {
					query = fmt.Sprintf("%s,\"%s\"", query, v.Field(i).String())
				}
			default:
				fmt.Println("不支持此类型")
				return
			}
		}
		query = fmt.Sprintf("%s)", query)
		fmt.Println(query)
		return
	} else {
		fmt.Println("不支持此类型")
	}
}

func main() {
	u := User{
		Id:       1,
		UserName: "kali",
		PassWord: "kali",
	}
	CreateQuery(u)
	r := Permission{
		RoleId: 1,
		Leve:   1,
		Des:    "超级管理员",
	}
	CreateQuery(r)

	t := 1
	CreateQuery(t)
}

```

我们会获得结果：

```bash
insert into user values(1,"kali","kali")
insert into permission values(1, 1,"超级管理员")
不支持此类型
```

这样我们就得到了复用，这样一个完整的实例应该让你对反射有了更深的理解了吧？其实不要被这个名字误解了，我们可以把它理解为对我们传参的数据进行拆解，让我们可以通过已知类型进行更多操作。

## 参考资料

[Go 系列教程 —— 34. 反射](https://studygolang.com/articles/13178)

[手把手教你学之golang反射](https://www.jianshu.com/p/c890ae5da90e)
