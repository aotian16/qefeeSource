title: gobyexample_channelsynchronization
date: 2014-03-10 22:08:43
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

**go 管道同步**

<!--more-->

<!--body-->

``` go
// GoByExample_ChannelSynchronization project main.go

// We can use channels to synchronize execution
// across goroutines. Here's an example of using a
// blocking receive to wait for a goroutine to finish.
// 在goroutines之间，我们可以通过channel执行同步。
// 这里有一个用一个阻塞接收数据，等待直到goroutine 完成。
package main

import "fmt"
import "time"

// This is the function we'll run in a goroutine. The
// `done` channel will be used to notify another
// goroutine that this function's work is done.
// 这里有一个我们将要运行在goroutine中的函数。
// done channel将被用来通知另外的goroutine这个函数已经工作完毕。
func worker(done chan bool) {
	fmt.Print("working...")
	time.Sleep(time.Second)
	fmt.Println("done")

	// Send a value to notify that we're done.
	// 发送一个值去通知他们我们已经完毕。
	done <- true
}

func main() {

	// Start a worker goroutine, giving it the channel to
	// notify on.
	// 开始一个工作的goroutine，给它一个可以去通知的channel。
	done := make(chan bool, 1)
	go worker(done)

	// Block until we receive a notification from the
	// worker on the channel.
	// 阻塞直到我们从这个工作的channel中接收到一个通知。
	<-done
}

// 输出
//working...done
```