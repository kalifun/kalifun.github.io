---
title: Golang并发(goroutine)
date: 2020-01-19 00:00:00
categories: Golang
tags:
  - golang
---

# Golang并发

`go`语言这么受欢迎的原因之一，我们只需要将`go`关键字放到函数前，这样就实现了并发；不需要考虑CPU的调度，这些`go`都在底层帮我们实现了。不要通过共享内存来通信，而应该通过通信来共享内存。

## 前言

通常我们都会听到并发，并行，进程，线程。这些概念总会被我们搞混淆，并傻傻分不清楚。下面我们再通过一些例子来说明：

`并发： 逻辑上具有处理多个同时性任务的能力。(在一段时间里，你与A和B在聊天)`

`并行：物理上同一时刻执行多个并发任务。(在同一时刻，A和B都与你聊天)`

`进程：具有一定独立功能的程序在一个数据集上的一次动态执行的过程。（我们可以理解进程就是一座矿山）`

`线程：程序执行中一个单一的顺序控制流程，是程序执行流的最小单元，是处理器调度和分派的基本单位。(而线程则是矿工，且最少不能少于1个)`

我们这里不会扩展去讲`Golang`调度器[`GMP模型`](https://www.cnblogs.com/sunsky303/p/9705727.html),文章结尾会给出推荐文章，感兴趣的可以进行学习。这里主要讲如何使用并发，及函数与函数之前是如何数据共享`channel`。

那就跟着我去了解`goroutine`,`channel`吧！

## goroutine

> 建议先阅读[Go语言基础之并发](https://www.liwenzhou.com/posts/Go/14_concurrence/)

`goroutine` 可以看作是轻量级线程。一个`goroutine`必定对应一个函数，可以创建多个`goroutine`去执行相同的函数。

下面我们加入`goroutine`实例：

```go
package main

import (
	"fmt"
	"time"
)

func hello() {
	fmt.Println("hello Goroutine!", time.Now())
}

func main() {
	go hello()
	fmt.Println("main goroutine done")
}
```

我们得到的结果是:

```bash
main goroutine done
```

肯定会好奇，怎么没有返回 hello 函数的值？因为创建`goroutine` 需要消耗时间。当我们准备返回值是，`main goroutibne`已经结束了。所以我们看不到返回值。

我们可以使用`time.sleep()`这个和我们其他语言不要一样。它的最小单位纳秒，所以不要直接填一个整数，这样可能还是无法正常返回值。

### WaitGroup

`Waitgroup` 用来等待一组`goroutines`的结束。

```bash
func (wg *WaitGroup) Add(delta int)
func (wg *WaitGroup) Done()
func (wg *WaitGroup) Wait()
```

在主`goroutine`里声明，并且Add要等待的`goroutine`的个数，每个`goroutine`执行完成之后调用 Done（每次完成就减一），最后在主`goroutines` 里 Wait 即可。`Add()` 方法一定要在 `goroutine `开始前执行。

下面我们结合`waitgroup`一起使用:

```go
package main

import (
	"fmt"
	"sync"
	"time"
)
// 声明一个waitgroup
var wg sync.WaitGroup

func hello(i int) {
    // 在函数结束前计数减一
	defer wg.Done()
	fmt.Println("hello Goroutine!", i, time.Now())
}

func main() {
	for i := 0; i < 10; i++ {
        // 设置需要等待goroutine的个数
		wg.Add(1)
		go hello(i)
	}
    //等待主goroutine结束
	wg.Wait()
}
```

我们得到的结果是:

```bash
$ go run main.go
hello Goroutine! 3 2020-01-20 11:06:27.257148 +0800 CST m=+0.004000001
hello Goroutine! 9 2020-01-20 11:06:27.257148 +0800 CST m=+0.004000001
hello Goroutine! 0 2020-01-20 11:06:27.256148 +0800 CST m=+0.003000001
hello Goroutine! 6 2020-01-20 11:06:27.257148 +0800 CST m=+0.004000001
hello Goroutine! 7 2020-01-20 11:06:27.257148 +0800 CST m=+0.004000001
hello Goroutine! 4 2020-01-20 11:06:27.257148 +0800 CST m=+0.004000001
hello Goroutine! 8 2020-01-20 11:06:27.257148 +0800 CST m=+0.004000001
hello Goroutine! 1 2020-01-20 11:06:27.256148 +0800 CST m=+0.003000001
hello Goroutine! 5 2020-01-20 11:06:27.257148 +0800 CST m=+0.004000001
hello Goroutine! 2 2020-01-20 11:06:27.256148 +0800 CST m=+0.003000001
```

你会发现它们的先后顺序不一致，那是因为我们使用的是并发，`goroutine`的调度是随机的。

如果我们学习并发只是为了使用`goroutine`那就太委屈它了，还有一个关键点`channel`.

## channel

> Channel是可以让一个goroutine发送特定的值到另外一个goroutine的通信机制。

### 声明

`channel`是一种类型，一种引用类型。

```
var 变量名 chan 类型
```

由于它是引用类型，所以必须初始化之后才能使用，否则会报错。

```
make(chan 元素类型, [缓冲大小])
缓冲大小是可选项。
```

### 操作

 我们可以理解`channel`只有三个概念：取值，传值，关闭。还有一个需要明白的是`channel`的操作只有`<-`.

#### 取值

顾名思义就是从通道中进行取值。

```
x := <- ch
<-ch       // 从ch中接收值，忽略结果
这里很好理解，我们从一个channel赋值给了另一个channel
```

#### 传值

将一些值传入一个channel中

```
ch <- 100
将100传入channel中
```

#### 关闭

我们通过调用内置的`close`函数来关闭通道。

```
close(ch)
```

只有在通知接收方`goroutine`所有的数据都发送完毕的时候才需要关闭通道。通道和打开文件不一样，关闭文件并不是必须的，因为通过可以被垃圾回收机制回收。

我们需要注意的是：

1. 对已经关闭的通道发送消息会导致panic。
2. 对已经关闭的通道取值会将其取空为止。
3. 对已经关闭的通道且无值进行取值的话会获取对应类型的空值。
4. 关闭一个已经关闭的通道会导致panic。

### 四种通道类型

#### 无缓冲通道

无缓冲意味需要排队，一个接着一个。属于阻塞通道，需要完整的走完流程，才会判定下一个传值是可以继续的。

```
package main

import "fmt"

func recv(c chan int) {
	msg := <-c
	fmt.Println("接收消息", msg)
}

func main() {
	ch := make(chan int)
	go recv(ch)
	ch <- 10
	fmt.Println("发送成功")
}
```

```
$ go run main.go
发送成功

$ go run main.go
接收消息 10
发送成功

$ go run main.go
发送成功
接收消息 10
```

如果你出现了一下那些情况，我们上面已经解释过了。

#### 管道

![](https://image.kalifun.top/upload/2001/5abd9e17ba1d2c6e.jpg)

通道可以用来连接`goroutine`，这样一个的输出是另一个输入。这就叫做管道。

```go
package main

import "fmt"

var receive chan string
var send chan string

func Receive() {
	newmsg := <-send
	receive <- newmsg
}

func Send() {
	send <- "发送消息"
}

func main() {
	receive = make(chan string)
	send = make(chan string)
	go Send()
	go Receive()

	getmsg := <-receive
	fmt.Println(getmsg)
}
```

其实这个很好理解，就是和取快递是一个性质。你拿取件码去取件，他会根据取件码把快递给你。

#### 单项通道类型

看着表里也可以很快速知道，就是我指定你只能接收你就只能接收。我指定你只能取值，那你只能取值。

那下面我们看看我们是如何限制它的：

```
package main

import "fmt"

// send 只允许接收不允许发送
// receive 只允许发送不允许接收
func Receive(send <-chan string, receive chan<- string) {
	newmsg := <-send
	receive <- newmsg
}

// send只允许发送不能接收
func Send(send chan<- string) {
	send <- "发送消息"
}

func main() {
	receive := make(chan string)
	send := make(chan string)
	go Send(send)
	go Receive(send, receive)

	getmsg := <-receive
	fmt.Println(getmsg)
}
```

`chan <- Type 只能发送的通道，可以发送不允许接收`

`<-chan Type 只能接收的通道，可以接收不允许发送`

#### 有缓冲通道

可以在使用make函数初始化通道的时候为其指定通道的容量。

```go
func main() {
	ch := make(chan int, 1) // 创建一个容量为1的有缓冲区通道
	ch <- 10
	fmt.Println("发送成功")
}
```

只要通道的容量大于零，那么该通道就是有缓冲的通道，通道的容量表示通道中能存放元素的数量。就像你小区的快递柜只有那么个多格子，格子满了就装不下了，就阻塞了，等到别人取走一个快递员就能往里面放一个。

### 工作池

在工作中我们通常会使用可以指定启动的`goroutine`数量–`worker pool`模式，控制`goroutine`的数量，防止`goroutine`泄漏和暴涨。

```go
package main

import (
	"fmt"
	"time"
)

func Worker(id int, jobs <-chan int, results chan<- int) {
	for job := range jobs {
		fmt.Printf("worker:%d start job:%d\n", id, job)
		time.Sleep(time.Second)
		fmt.Printf("worker:%d end job:%d\n", id, job)
		results <- job * 2
	}
}

func main() {
	jobs := make(chan int, 100)
	results := make(chan int, 100)

	// 开启多个goroutine
	for work := 1; work <= 2; work++ {
		go Worker(work, jobs, results)
	}
	for j := 1; j <= 5; j++ {
		jobs <- j
	}
	close(jobs)

	for a := 1; a <= 5; a++ {
		<-results
	}
}
```

```
$ go run main.go
worker:2 start job:1
worker:1 start job:2
worker:1 end job:2
worker:1 start job:3
worker:2 end job:1
worker:2 start job:4
worker:2 end job:4
worker:1 end job:3
worker:2 start job:5
worker:2 end job:5
```

