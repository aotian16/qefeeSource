title: GoByExample_Values
date: 2014-03-04 20:58:29
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

**go值类型**

<!--more-->

<!--body-->

``` go
// GoByExample_Values project main.go
// Go has various value types including strings,
// integers, floats, booleans, etc. Here are a few
// basic examples.

package main

import "fmt"

func main() {

	// Strings, which can be added together with `+`.
	fmt.Println("go" + "lang") // 字符型，用"+"号连接

	// Integers and floats.
	fmt.Println("1+1 =", 1+1)         // 整数
	fmt.Println("7.0/3.0 =", 7.0/3.0) // 浮点数

	// Booleans, with boolean operators as you'd expect.
	fmt.Println(true && false) // 布尔型，与
	fmt.Println(true || false) // 布尔型，或
	fmt.Println(!true)         // 布尔型，非
}

// 输出
//golang
//1+1 = 2
//7.0/3.0 = 2.3333333333333335
//false
//true
//false

```