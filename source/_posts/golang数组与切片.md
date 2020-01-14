---
title: golang数组与切片
date: 2020-01-13 15:24:00
categories: Golang
tags:
  - golang
---

# golang数组与切片

声明数组，切片类型的基本语法：

>数组： var 数组变量名 [元素数量]元素类型
>
>切片： var 变量名 []元素类型

单纯看上面的类型，你会发现它们很相似。没错切片就是基于数组类型做的一层封装。让它更加灵活，支持自动扩容。下面我们利用代码来区分它们的不同之处。

## 声明

### 数组初始化

我们将列举常见的声明形式：

```go
func main() {
	// 不进行初始化值，但程序会帮我们初始化值
	var arr1 [3]int
	fmt.Printf("arr1的类型:%T,arr1的值:%v\n", arr1, arr1)
	// 指定初始化值，完成初始化
	var arr2 = [3]string{"test", "test1"}
	fmt.Printf("arr2的类型:%T,arr2的值:%v\n", arr2, arr2)
	// 通过编译器根据初始化值进行推断数组长度
	var arr3 = [...]int{1, 2, 3, 4, 5}
	fmt.Printf("arr3的类型:%T,arr3的值:%v\n", arr3, arr3)
	// 指定索引的值進行初始化
	var arr4 = [...]int{1: 1, 6: 6}
	fmt.Printf("arr4的类型:%T,arr4的值:%v\n", arr4, arr4)
}
```

```bash
arr1的类型:[3]int,arr1的值:[0 0 0]
arr2的类型:[3]string,arr2的值:[test test1 ]
arr3的类型:[5]int,arr3的值:[1 2 3 4 5]
arr4的类型:[7]int,arr4的值:[0 1 0 0 0 0 6]
```

### 切片初始化

我们将列举常见的声明形式：

```go
func main() {
	// 声明一个字符串切片
	var slice1 []string
	fmt.Printf("slice1类型:%T,slice1的值:%v\n", slice1, slice1)
	// 声明布尔类型的切片，并初始化值
	var slice2 = []bool{true, false}
	fmt.Printf("slice2类型:%T,slice2的值:%v\n", slice2, slice2)
	// 基于数组定义切片
	array1 := [5]int{1, 2, 3, 4, 5}
	// 基于数组array1创建切片
	slice3 := array1[1:3]
	fmt.Printf("slice3类型:%T,slice3的值:%v\n", slice3, slice3)
	// 使用make()函数创建切片
	// make([]类型,切片中的元素数量,切片的容量)
	slice4 := make([]int, 2, 10)
	fmt.Printf("slice4类型:%T,slice4的值:%v\n", slice4, slice4)
	fmt.Printf("slice3长度:%v\n", len(slice4))
	fmt.Printf("slice3容量:%v\n", cap(slice4))
}
```

```bash
slice1类型:[]string,slice1的值:[]
slice2类型:[]bool,slice2的值:[true false]
slice3类型:[]int,slice3的值:[2 3]
slice4类型:[]int,slice4的值:[0 0]
slice3长度:2
slice3容量:10
```

从这里应该可以看出一点小差异，数组需要指定元素长度。而切片有容量的说法，可以对长度进行扩展。

## 差异

下面列举一些实际运用场景的不同方式。

### 遍历

#### 数组遍历

```go
func main() {
	// 第一种遍历方式
	var array = [...]string{"上海", "北京", "深圳", "广州"}
	for i := 0; i < len(array); i++ {
		fmt.Println(array[i])
	}

	// 第二种遍历方式
	for index, value := range array {
		fmt.Println(index, " ", value)
	}
}
```

```bash
	// 上海
	// 北京
	// 深圳
	// 广州
	// 0   上海
	// 1   北京
	// 2   深圳
	// 3   广州
```

#### 切片遍历

```go
func main() {
	slice := []int{1, 2, 3, 4, 5}

	// 第一种遍历方式
	for i := 0; i < len(slice); i++ {
		fmt.Println(slice[i])
	}

	// 第二种遍历方式
	for index, value := range slice {
		fmt.Println(index, value)
	}
}
```

```bash
	// 1
	// 2
	// 3
	// 4
	// 5
	// 0 1
	// 1 2
	// 2 3
	// 3 4
	// 4 5
```

你会发现数组和切片遍历是没有区别。方式都是一致的，源于切片是基于数组进行封装的。

### 添加元素

数组是不包含这个功能的，你可以理解数组是静态数组，无法添加新的元素，而切片是动态数组，可以追加元素。

```go
func main() {
	var slice []int
	for i := 0; i < 8; i++ {
		slice = append(slice, i)
		fmt.Printf("%v len:%d cap:%d prt:%p\n", slice, len(slice), cap(slice), slice)
	}
}
```

```bash
	// [0] len:1 cap:1 prt:0xc000012090
	// [0 1] len:2 cap:2 prt:0xc0000120d0
	// [0 1 2] len:3 cap:4 prt:0xc00009e000
	// [0 1 2 3] len:4 cap:4 prt:0xc00009e000
	// [0 1 2 3 4] len:5 cap:8 prt:0xc0000a4000
	// [0 1 2 3 4 5] len:6 cap:8 prt:0xc0000a4000
	// [0 1 2 3 4 5 6] len:7 cap:8 prt:0xc0000a4000
	// [0 1 2 3 4 5 6 7] len:8 cap:8 prt:0xc0000a4000
```

你仔细看cap，你会发现一个规律，它是2的倍数进行递增的。当len大于cap时，cap将会进行扩容。

### 复制切片

我们先看代码在对其进行分析：

```go
func main() {
	var slice = []int{1, 2, 3, 4, 5, 6, 7}
	slice2 := slice
	fmt.Println("这是slice:", slice)
	fmt.Println("这是slice2:", slice2)
	slice2[0] = 1000
	fmt.Println("这是新的slice:", slice)
	fmt.Println("这是新的slice2:", slice2)
}
```

```bash
	// 这是slice: [1 2 3 4 5 6 7]
	// 这是slice2: [1 2 3 4 5 6 7]
	// 这是新的slice: [1000 2 3 4 5 6 7]
	// 这是新的slice2: [1000 2 3 4 5 6 7]
```

当我们对复制的切片进行修改其值时，你会发现源切片的值也被修改了，说明当复制时，地址指针还是指向源切片地址，所以对任何一个切片进行修改，我们的值都会改变。

### 切片删除元素

其实切片没有有删除元素的函数，但我们可以利用append来实现删除元素的功能。

```go
func main() {
	var slice = []int{1, 2, 3, 4, 5, 6}
	// 需要删除索引为3的元素
	slice = append(slice[:3], slice[4:]...)
	fmt.Println(slice)
}
```

```bash
	//[1 2 3 5 6]
```

