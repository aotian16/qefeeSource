title: gobyexample_timeouts
date: 2014-03-11 23:24:19
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
// GoByExample_Timeouts project main.go

// _Timeouts_ are important for programs that connect to
// external resources or that otherwise need to bound
// execution time. Implementing timeouts in Go is easy and
// elegant thanks to channels and `select`.
// 超时处理在连接到外部资源时候是非常重要的程序，
// 否则就需要绑定执行时间了。
// 在go中由于有channel和select超时处理是简单和优雅的。

package main

import "time"
import "fmt"

func main() {

	// For our example, suppose we're executing an external
	// call that returns its result on a channel `c1`
	// after 2s.
	// 对于我们的例子，假设我们有个外部调用，
	// 它2秒后通过c1这个channel返回。
	c1 := make(chan string, 1)
	go func() {
		time.Sleep(time.Second * 2)
		c1 <- "result 1"
	}()

	// Here's the `select` implementing a timeout.
	// `res := <-c1` awaits the result and `<-Time.After`
	// awaits a value to be sent after the timeout of
	// 1s. Since `select` proceeds with the first
	// receive that's ready, we'll take the timeout case
	// if the operation takes more than the allowed 1s.
	// 这里select实现了超时处理。res := <-c1 正在等待返回结果。
	// <-Time.After 正在等待1s后发送来的值。
	// 由于select只执行第一个就绪的返回。
	// 我们将在执行时间超过1秒的情况下做超时处理。
	select {
	case res := <-c1:
		fmt.Println(res)
	case <-time.After(time.Second * 1):
		fmt.Println("timeout 1")
	}

	// If we allow a longer timeout of 3s, then the receive
	// from `c2` will succeed and we'll print the result.
	// 如果我们允许超时时间是3秒，我们将成功的从c2返回并打印结果。
	c2 := make(chan string, 1)
	go func() {
		time.Sleep(time.Second * 2)
		c2 <- "result 2"
	}()
	select {
	case res := <-c2:
		fmt.Println(res)
	case <-time.After(time.Second * 3):
		fmt.Println("timeout 2")
	}
}

// todo: cancellation?

// 输出
//timeout 1
//result 2

```