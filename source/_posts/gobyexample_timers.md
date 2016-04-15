title: gobyexample_timers
date: 2014-03-13 19:53:19
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

**go timers**

<!--more-->

<!--body-->

``` go
// GoByExample_Timers project main.go

// We often want to execute Go code at some point in the
// future, or repeatedly at some interval. Go's built-in
// _timer_ and _ticker_ features make both of these tasks
// easy. We'll look first at timers and then
// at [tickers](tickers).
// 我们经常需要在将来的某个时间点，或者重复的时间间隔执行代码。
// Go语言中内置的timer和ticker特性使得两者都变得十分简单。
// 我们先看timer再看tickers。

package main

import "time"
import "fmt"

func main() {

	// Timers represent a single event in the future. You
	// tell the timer how long you want to wait, and it
	// provides a channel that will be notified at that
	// time. This timer will wait 2 seconds.
	//   Timer表现为将来的一个简单事件。
	// 你告诉timer你需要等待多出长时间，它就会产生一个channel，
	// 然后到时候通知给这个channel。这里的timer将等待2秒。

	timer1 := time.NewTimer(time.Second * 2)

	// The `<-timer1.C` blocks on the timer's channel `C`
	// until it sends a value indicating that the timer
	// expired.
	// <-timer1.C 阻塞了timer的channel C直到它被告知时间已经到了。
	<-timer1.C
	fmt.Println("Timer 1 expired")

	// If you just wanted to wait, you could have used
	// `time.Sleep`. One reason a timer may be useful is
	// that you can cancel the timer before it expires.
	// Here's an example of that.
	//  如果你仅仅只是需要等待一段时间，你可以用time.Sleep。
	// 一个原因是timer可以在过期时间到之前取消掉的。这里就有这样的一个例子。
	timer2 := time.NewTimer(time.Second)
	go func() {
		<-timer2.C
		fmt.Println("Timer 2 expired")
	}()
	stop2 := timer2.Stop()
	if stop2 {
		fmt.Println("Timer 2 stopped")
	}
}

// 输出
//Timer 1 expired
//Timer 2 stopped

```