title: gobyexample_select
date: 2014-03-11 23:16:19
tags: gobyexample
categories: go
---

<!--head-->

========

**参考网站**

* [gobyexample](https://gobyexample.com/ "gobyexample")
* [go语言简单示例(gobyexample翻译)](http://bbs.csdn.net/topics/390557446 "go语言简单示例")
* [启飞(gobyexample源码注释版)](http://qefee.com/tags/gobyexample/ "启飞")

========

**go select**

<!--more-->

<!--body-->

``` go
// GoByExample_Select project main.go

// Go's _select_ lets you wait on multiple channel
// operations. Combining goroutines and channels with
// select is a powerful feature of Go.
// go语言select可以让你等待多个channel的操作。
// 把goroutines和channels结合在一起是一个强大特性。

package main

import "time"
import "fmt"

func main() {

	// For our example we'll select across two channels.
	// 例如我们将要通过两个channel来运用select。
	c1 := make(chan string)
	c2 := make(chan string)

	// Each channel will receive a value after some amount
	// of time, to simulate e.g. blocking RPC operations
	// executing in concurrent goroutines.
	// 每一个channel都将会在一定时候后，接受一个值，
	// 来模拟prc或者并发的goroutines。
	go func() {
		time.Sleep(time.Second * 1)
		c1 <- "one"
	}()
	go func() {
		time.Sleep(time.Second * 2)
		c2 <- "two"
	}()

	// We'll use `select` to await both of these values
	// simultaneously, printing each one as it arrives.
	// 我们将用select等待这些值，并在他们到达的时候打印他们。
	for i := 0; i < 2; i++ {
		select {
		case msg1 := <-c1:
			fmt.Println("received", msg1)
		case msg2 := <-c2:
			fmt.Println("received", msg2)
		}
	}
}

// 输出
//received one
//received two

```