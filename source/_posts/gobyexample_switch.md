title: GoByExample_Switch
date: 2014-03-04 20:58:34
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

**go switch语句**

<!--more-->

<!--body-->

``` go
// GoByExample_Switch project main.go

// _Switch statements_ express conditionals across many
// branches.
// go语言专门用于多条件分支语句。

package main

import "fmt"
import "time"

func main() {

	// Here's a basic `switch`.
	// 这里有个基本的switch语句。
	i := 2
	fmt.Print("write ", i, " as ")
	switch i {
	case 1:
		fmt.Println("one")
	case 2:
		fmt.Println("two")
	case 3:
		fmt.Println("three")
	}

	// You can use commas to separate multiple expressions
	// in the same `case` statement. We use the optional
	// `default` case in this example as well.
	// 在同一个case语句中，可以用逗号分隔不同的条件。
	// 在这个例子中，我们使用默认的default语句。
	switch time.Now().Weekday() {
	case time.Saturday, time.Sunday:
		fmt.Println("it's the weekend")
	default:
		fmt.Println("it's a weekday")
	}

	// `switch` without an expression is an alternate way
	// to express if/else logic. Here we also show how the
	// `case` expressions can be non-constants.
	// 没有表达式的switch语句可以替代if/else语句。
	// 这里我们显示了case表达式可以不是常量。
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("it's before noon")
	default:
		fmt.Println("it's after noon")
	}
}

// 输出(由电脑时间决定)
//write 2 as two
//it's a weekday
//it's after noon

// todo: type switches

```