title: GoByExample_Ifelse
date: 2014-03-04 20:58:33
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

**go if/else语句**

<!--more-->

<!--body-->

``` go
// GoByExample_Ifelse project main.go

// Branching with `if` and `else` in Go is
// straight-forward.
// if else分支在go语言里是简单明了的。

package main

import "fmt"

func main() {

	// Here's a basic example.
	// 这里有个简单的例子。
	if 7%2 == 0 {
		fmt.Println("7 is even") // 偶数
	} else {
		fmt.Println("7 is odd") // 奇数
	}

	// You can have an `if` statement without an else.
	// if语句可以没有else。
	if 8%4 == 0 {
		fmt.Println("8 is divisible by 4") // 整除
	}

	// A statement can precede conditionals; any variables
	// declared in this statement are available in all
	// branches.
	// 可以在条件语句前添加别的语句。这个语句中声明的变量的作用域在所有的分支中。
	if num := 9; num < 0 {
		fmt.Println(num, "is negative")
	} else if num < 10 {
		fmt.Println(num, "has 1 digit")
	} else {
		fmt.Println(num, "has multiple digits")
	}
}

// Note that you don't need parentheses around conditions
// in Go, but that the brackets are required.
// 注意：在go语言中条件语句没必要带圆括号。但是分支中必须有大括号。

// 输出
//7 is odd
//8 is divisible by 4
//9 has 1 digit

```