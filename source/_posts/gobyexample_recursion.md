title: GoByExample_Recursion
date: 2014-03-04 20:58:43
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

**go 递归**

<!--more-->

<!--body-->

``` go
// GoByExample_Recursion project main.go

// Go supports
// <a href="http://en.wikipedia.org/wiki/Recursion_(computer_science)"><em>recursive functions</em></a>.
// Here's a classic factorial example.
// go语言支持递归函数。这里有个典型的阶乘的例子。

package main

import "fmt"

// This `fact` function calls itself until it reaches the
// base case of `fact(0)`.
// fac函数调用它自己直到到达他基本的情况face(0)。
func fact(n int) int {
	if n == 0 {
		return 1
	}
	return n * fact(n-1)
}

func main() {
	fmt.Println(fact(7))
}

// 输出
//5040

```