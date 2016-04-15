title: GoByExample_MultipleReturnValues
date: 2014-03-04 20:58:40
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

**go 函数的多返回值**

<!--more-->

<!--body-->

``` go
// GoByExample_MultipleReturnValues project main.go

// Go has built-in support for _multiple return values_.
// This feature is used often in idiomatic Go, for example
// to return both result and error values from a function.
// go语言内置支持多返回值。
// 这个特性在go语言中是个常用的习惯，例如在函数里同时返回结果和错误。
package main

import "fmt"

// The `(int, int)` in this function signature shows that
// the function returns 2 `int`s.
// 函数中的(int, int)签名表示这个函数会返回2个整形。
func vals() (int, int) {
	return 3, 7
}

func main() {

	// Here we use the 2 different return values from the
	// call with _multiple assignment_.
	// 这里我们将2个不同的返回值分配给多个变量。
	a, b := vals()
	fmt.Println(a)
	fmt.Println(b)

	// If you only want a subset of the returned values,
	// use the blank identifier `_`.
	// 如果你只想要返回值中子集，那么请使用“_”标识符。
	_, c := vals()
	fmt.Println(c)
}

// todo: named return parameters
// todo: naked returns

// 输出
//3
//7
//7

```