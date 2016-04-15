title: gobyexample_tickers
date: 2014-03-13 20:03:54
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

**go tickers**

<!--more-->

<!--body-->

``` go
// GoByExample_Tickers project main.go

// [Timers](timers) are for when you want to do
// something once in the future - _tickers_ are for when
// you want to do something repeatedly at regular
// intervals. Here's an example of a ticker that ticks
// periodically until we stop it.
// timer用在你想让程序过段时间做点事情的时候，
// 而ticker用在你想让程序间隔一段时间重复的做某事。
// 这里有一个ticket的例子，它会不断的"嘀嗒"直到我们停止它。

package main

import "time"
import "fmt"

func main() {

	// Tickers use a similar mechanism to timers: a
	// channel that is sent values. Here we'll use the
	// `range` builtin on the channel to iterate over
	// the values as they arrive every 500ms.
	// ticker用和timer一样的机制：用一个channel发送值。
	// 我们将用channe内建的range来遍历其每500ms接收的值。
	ticker := time.NewTicker(time.Millisecond * 500)
	go func() {
		for t := range ticker.C {
			fmt.Println("Tick at", t)
		}
	}()

	// Tickers can be stopped like timers. Once a ticker
	// is stopped it won't receive any more values on its
	// channel. We'll stop ours after 1500ms.
	// Ticker能像timer一样被停止。一旦一个ticker被停止了，
	// 它的channel就不会再接收值了。我们将停止我们的在1500ms后。
	time.Sleep(time.Millisecond * 1500)
	ticker.Stop()
	fmt.Println("Ticker stopped")
}

// 输出
//Tick at 2014-03-13 20:03:01.1269379 +0900 +0900
//Tick at 2014-03-13 20:03:01.6261856 +0900 +0900
//Tick at 2014-03-13 20:03:02.1266821 +0900 +0900
//Ticker stopped

```