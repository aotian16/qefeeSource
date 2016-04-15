title: GoByExample_Constants
date: 2014-03-04 20:58:31
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

**go常量**

<!--more-->

<!--body-->

``` go
// GoByExample_Constants project main.go

// Go supports _constants_ of character, string, boolean,
// and numeric values.
// go语言支持的常量有字符型，字符串型，布尔型和数字型。

package main

import "fmt"
import "math"

// `const` declares a constant value.
// 用const关键字来定义常量。
const s string = "constant"

func main() {
	fmt.Println(s)

	// A `const` statement can appear anywhere a `var`
	// statement can.
	// 能有var语句的地方，就能有const语句。
	const n = 500000000

	// Constant expressions perform arithmetic with
	// arbitrary precision.
	// 常量表达式能以任意精度进行计算。
	const d = 3e20 / n
	fmt.Println(d)

	// A numeric constant has no type until it's given
	// one, such as by an explicit cast.
	// 常量是没有类型的，除非有语句给出了，例如强制类型转换。
	fmt.Println(int64(d))

	// A number can be given a type by using it in a
	// context that requires one, such as a variable
	// assignment or function call. For example, here
	// `math.Sin` expects a `float64`.
	// 数字常量在上下文需要类型时候，其类型会被自动加上。
	// 比如说变量赋值或者函数调用中。
	// 本示例中的math.Sin(n)，sin
	// 函数需要一个int64类型，n会被自动转换成int64.
	fmt.Println(math.Sin(n))

	// We’ll see some other for forms later when we
	// look at range statements, channels, and other
	// data structures.
	// 当我们学习到range语句，channels，和其他的数据结构时候，
	// 我们会看到for语句的另外的形式。
}

// 输出
//constant
//6e+11
//600000000000
//-0.2847040732381611

```