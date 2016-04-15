title: gobyexample_channels
date: 2014-03-09 22:50:40
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

**go 管道**

<!--more-->

<!--body-->

``` go
// GoByExample_Channels project main.go

// _Channels_ are the pipes that connect concurrent
// goroutines. You can send values into channels from one
// goroutine and receive those values into another
// goroutine.
// Channel是一个连接当前goroutines的管道。
// 你可以在一个Channel中发送数据另一个中接收数据。他是一个原始的且功能强大的go功能。

package main

import "fmt"

func main() {

	// Create a new channel with `make(chan val-type)`.
	// Channels are typed by the values they convey.
	// 创建一个新的Channel可以用make(chan val-type).
	// 其类型通过传递的val-type的类型定义。
	messages := make(chan string)

	// _Send_ a value into a channel using the `channel <-`
	// syntax. Here we send `"ping"`  to the `messages`
	// channel we made above, from a new goroutine.
	// 发送一个值到一个channel，用channel <-语法。
	// 这里我们从新的gorountins发送了ping字符串
	// 到上面我定义的message channel。
	go func() { messages <- "ping" }()

	// The `<-channel` syntax _receives_ a value from the
	// channel. Here we'll receive the `"ping"` message
	// we sent above and print it out.
	// 语法<-channel 从channel中接收一个值。
	// 这里我们接收上面发送的ping字符串，并打印出来。
	msg := <-messages
	fmt.Println(msg)
}

```