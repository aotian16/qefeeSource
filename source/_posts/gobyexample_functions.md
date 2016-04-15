title: GoByExample_Functions
date: 2014-03-04 20:58:39
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

**go 函数**

<!--more-->

<!--body-->

``` go
// GoByExample_Functions project main.go

// _Functions_ are central in Go. We'll learn about
// functions with a few different examples.
// 函数是go语言的一个核心。我们通过一些不同的示例来学习函数。
package main

import "fmt"

// Here's a function that takes two `int`s and returns
// their sum as an `int`.
// 这里有一个有两个整数类型的参数，并返回它们整数型的和的函数。
// *注意返回值类型在后面
func plus(a int, b int) int {

	// Go requires explicit returns, i.e. it won't
	// automatically return the value of the last
	// expression.
	// go语言的函数需要显示返回，即它不能自动返回最后一个表达式的值。
	return a + b
}

func main() {

	// Call a function just as you'd expect, with
	// `name(args)`.
	// 正如你所期望的那样，调用函数用name(args)这种方式.
	res := plus(1, 2)
	fmt.Println("1+2 =", res)
}

// todo: coalesced parameter types

// 输出
//1+2 = 3

```