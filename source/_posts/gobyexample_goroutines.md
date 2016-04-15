title: gobyexample_goroutines
date: 2014-03-09 22:40:40
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

**go goroutines**

<!--more-->

<!--body-->

``` go
// GoByExample_Goroutines project main.go

// A _goroutine_ is a lightweight thread of execution.
// Goroutine 是一个轻量级的执行线程。
package main

import "fmt"

func f(from string) {
	for i := 0; i < 3; i++ {
		fmt.Println(from, ":", i)
	}
}

func main() {

	// Suppose we have a function call `f(s)`. Here's how
	// we'd call that in the usual way, running it
	// synchronously.
	// 假设我们有一个函数f(s)。这里有一个我们以前常用的同步调用方式。
	f("direct")

	// To invoke this function in a goroutine, use
	// `go f(s)`. This new goroutine will execute
	// concurrently with the calling one.
	// 要在 goroutine 中调用此函数，使用 go f(s)。
	// 这个新的 goroutine 将调用一个并发执行。
	go f("goroutine")

	// You can also start a goroutine for an anonymous
	// function call.
	// 你也可以在goroutine中用匿名函数。
	go func(msg string) {
		fmt.Println(msg)
	}("going")

	// Our two goroutines are running asynchronously in
	// separate goroutines now, so execution falls through
	// to here. This `Scanln` code requires we press a key
	// before the program exits.
	// 我们两个的goroutine是异步运行的，
	// 因此在单独的goroutine执行落在这里
	//（我自己的理解是，goroutine是异步的,
	// 所以其运行其实是在第一个非go语句之后才开始的）。
	// 此Scanln代码在程序退出之前，要求我们按一个键。
	var input string
	fmt.Scanln(&input)
	fmt.Println("done")
}

```