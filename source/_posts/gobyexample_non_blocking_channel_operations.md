title: gobyexample_non_blocking_channel_operations
date: 2014-03-12 20:35:41
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

**go 非阻塞管道操作**

<!--more-->

<!--body-->

``` go
// GoByExample_NonBlockingChannelOperations project main.go

// Basic sends and receives on channels are blocking.
// However, we can use `select` with a `default` clause to
// implement _non-blocking_ sends, receives, and even
// non-blocking multi-way `select`s.
// channel的基本操作，接收和发送都是阻塞的。
// 但是我们能用select加上其default条件来实现非阻塞的发送，
// 接收甚至多路非阻塞。

package main

import "fmt"

func main() {
	messages := make(chan string)
	signals := make(chan bool)

	// Here's a non-blocking receive. If a value is
	// available on `messages` then `select` will take
	// the `<-messages` `case` with that value. If not
	// it will immediately take the `default` case.
	// 这里有个非阻塞的接收。
	// 如果message有一个值，那么select将通过<-messages
	// 这个case语句来接收它，否则他将马上执行default语句。
	select {
	case msg := <-messages:
		fmt.Println("received message", msg)
	default:
		fmt.Println("no message received")
	}

	// A non-blocking send works similarly.
	// 一个非阻塞的发送同样这样工作。
	msg := "hi"
	select {
	case messages <- msg:
		fmt.Println("sent message", msg)
	default:
		fmt.Println("no message sent")
	}

	// We can use multiple `case`s above the `default`
	// clause to implement a multi-way non-blocking
	// select. Here we attempt non-blocking receives
	// on both `messages` and `signals`.
	// 我们能用多个case 在default子句上面来实现select的多路非阻塞。
	// 这里我们尝试多路的接收message和signals。
	select {
	case msg := <-messages:
		fmt.Println("received message", msg)
	case sig := <-signals:
		fmt.Println("received signal", sig)
	default:
		fmt.Println("no activity")
	}
}

// 输出
//no message received
//no message sent
//no activity

```