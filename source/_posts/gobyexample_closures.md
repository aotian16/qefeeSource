title: GoByExample_Closures
date: 2014-03-04 20:58:42
tags: gobyexample
categories: go
---

<!--head-->

**参考网站**

* [gobyexample](https://gobyexample.com/ "gobyexample")
* [go语言简单示例(gobyexample翻译)](http://bbs.csdn.net/topics/390557446 "go语言简单示例")
* [启飞(gobyexample源码注释版)](http://qefee.com/tags/gobyexample/ "启飞")

========

**go 闭包**

<!--more-->

``` go
// GoByExample_Closures project main.go

// Go supports [_anonymous functions_](http://en.wikipedia.org/wiki/Anonymous_function),
// which can form <a href="http://en.wikipedia.org/wiki/Closure_(computer_science)"><em>closures</em></a>.
// Anonymous functions are useful when you want to define
// a function inline without having to name it.
// go语言支持闭包的一种形式，匿名函数。
// 匿名函数在你希望用一行代码定义一个没有名字的函数的时候有用。

package main

import "fmt"

// This function `intSeq` returns another function, which
// we define anonymously in the body of `intSeq`. The
// returned function _closes over_ the variable `i` to
// form a closure.
func intSeq() func() int {
	i := 0
	return func() int {
		i += 1
		return i
	}
}

func main() {

	// We call `intSeq`, assigning the result (a function)
	// to `nextInt`. This function value captures its
	// own `i` value, which will be updated each time
	// we call `nextInt`.
	// 函数intSeq将返回另外一个函数。
	// 这个函数是我们在他的函数体内定义的匿名函数。
	// 它关闭了变量i形成了个闭包。
	// 我们调用intSeq，把他的返回值（另外一个函数）赋值给变量nextInt.
	// 这个函数会捕获他自己的i的值，并在每次调用nextInt的时候，更新该值。
	nextInt := intSeq()

	// See the effect of the closure by calling `nextInt`
	// a few times.
	// 来我们看看几次调用闭包nextInt的影响。
	fmt.Println(nextInt())
	fmt.Println(nextInt())
	fmt.Println(nextInt())

	// To confirm that the state is unique to that
	// particular function, create and test a new one.
	// 为了确认闭包的状态是唯一的，我们创建了了一个新的测试。
	newInts := intSeq()
	fmt.Println(newInts())
}

// 输出
//1
//2
//3
//1

```