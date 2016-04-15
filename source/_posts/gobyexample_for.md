title: GoByExample_For
date: 2014-03-04 20:58:32
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

**go for语句**

<!--more-->

<!--body-->

``` go
// GoByExample_For project main.go

// `for` is Go's only looping construct. Here are
// three basic types of `for` loops.
// go语言唯一的循环结构是for语句。这里有三种基本的for循环结构。

package main

import "fmt"

func main() {

	// The most basic type, with a single condition.
	// 最简单的形式是只有一个单一的条件语句。
	// (相当于一些语言的while)
	i := 1
	for i <= 3 {
		fmt.Println(i)
		i = i + 1
	}

	// A classic initial/condition/after `for` loop.
	// 一个典型的for语句是具有初始化语句，条件语句，和执行后语句的。
	for j := 7; j <= 9; j++ {
		fmt.Println(j)
	}

	// `for` without a condition will loop repeatedly
	// until you `break` out of the loop or `return` from
	// the enclosing function.
	// for语句如果没有条件语句会一直循环直到有break语句
	// 或者return语句返回的时候。
	// (即死循环)
	for {
		fmt.Println("loop")
		break
	}
}

// 输出
//1
//2
//3
//7
//8
//9
//loop
```