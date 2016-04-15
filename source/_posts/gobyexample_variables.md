title: GoByExample_Variables
date: 2014-03-04 20:58:30
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

**go变量**

<!--more-->

<!--body-->

``` go
// GoByExample_Variables project main.go

// In Go, _variables_ are explicitly declared and used by
// the compiler to e.g. check type-correctness of function
// calls.

// 在go语言中，变量都的声明都是显式的。编译器会用他们来做例如类型检查等事情。

package main

import "fmt"

func main() {

	// `var` declares 1 or more variables.
	// 用var声明1个或者多个变量。
	var a string = "initial"
	fmt.Println(a)

	// You can declare multiple variables at once.
	// 可以一次声明多个变量。
	var b, c int = 1, 2
	fmt.Println(b, c)

	// Go will infer the type of initialized variables.
	// go会对初始化的变量进行类型推断。
	var d = true
	fmt.Println(d)

	// Variables declared without a corresponding
	// initialization are _zero-valued_. For example, the
	// zero value for an `int` is `0`.
	// 声明的变量，如果没有初始化，那么其值为相应的零值。
	// 例如一个int类型的变量其零值就是0。
	var e int // e会初始化为0
	fmt.Println(e)

	// The `:=` syntax is shorthand for declaring and
	// initializing a variable, e.g. for
	// `var f string = "short"` in this case.
	// ":="语句是声明并初始化的简写形式。例如这里的 var f string = "short"。
	// 这样可以省略var和类型，go会自己推导类型
	f := "short"
	fmt.Println(f)
}

// 输出
//initial
//1 2
//true
//0
//short

```