title: gobyexample_channeldirections
date: 2014-03-10 23:07:14
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

**go 管道方向**

<!--more-->

<!--body-->

``` go
// GoByExample_ChannelDirections project main.go

// When using channels as function parameters, you can
// specify if a channel is meant to only send or receive
// values. This specificity increases the type-safety of
// the program.
// 当用chennels 作为一个函数的参数时候，
// 我们可以指定他只是发送数据还是接收数据。
// 这种方式有助于提高程序的类型安全性。

package main

import "fmt"

// This `ping` function only accepts a channel for sending
// values. It would be a compile-time error to try to
// receive on this channel.
// 函数ping定义了一个发送数据的channel。
// 如果用这个channel接收数据，将会导致一个编译错误。
func ping(pings chan<- string, msg string) {
	pings <- msg
}

// The `pong` function accepts one channel for receives
// (`pings`) and a second for sends (`pongs`).
// 函数pong的第一个参数是一个接收数据的channel（pings），
// 第二个是发送的（pongs）。
func pong(pings <-chan string, pongs chan<- string) {
	msg := <-pings
	pongs <- msg
}

func main() {
	pings := make(chan string, 1)
	pongs := make(chan string, 1)
	ping(pings, "passed message")
	pong(pings, pongs)
	fmt.Println(<-pongs)
}

// pings chan<- string表示只发送通道
// pings <-chan string表示只接收通道

// 输出
//passed message

```