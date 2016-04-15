title: gobyexample_channelbuffering
date: 2014-03-10 21:58:30
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

**go 管道缓冲**

<!--more-->

<!--body-->

``` go
// GoByExample_ChannelBuffering project main.go

// By default channels are _unbuffered_, meaning that they
// will only accept sends (`chan <-`) if there is a
// corresponding receive (`<- chan`) ready to receive the
// sent value. _Buffered channels_ accept a limited
// number of  values without a corresponding receiver for
// those values.
// 默认情况下，channels 是没有缓冲的，
// 意味着他们只有当有一个对应的接受者的时候channel才会
// 接收一个发送者发送数据。有缓冲的channel接受有限的值，
// 这些值没有相应的接收器。

package main

import "fmt"

func main() {

	// Here we `make` a channel of strings buffering up to
	// 2 values.
	// 这里我构建了有两个字符串缓冲的channels。
	messages := make(chan string, 2)

	// Because this channel is buffered, we can send these
	// values into the channel without a corresponding
	// concurrent receive.
	// 由于这个channel是有缓冲的，
	// 我们能在没有对应接收者的情况下发送这些值给chennel。
	messages <- "buffered"
	messages <- "channel"

	// Later we can receive these two values as usual.
	// 然后，我能照以前样接收这两个值。
	fmt.Println(<-messages)
	fmt.Println(<-messages)
}

```