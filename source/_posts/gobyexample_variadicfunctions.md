title: GoByExample_VariadicFunctions
date: 2014-03-04 20:58:41
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

**go 函数的可变参数**

<!--more-->

<!--body-->

``` go
// GoByExample_VariadicFunctions project main.go

// [_Variadic functions_](http://en.wikipedia.org/wiki/Variadic_function)
// can be called with any number of trailing arguments.
// For example, `fmt.Println` is a common variadic
// function.
// 可变参函数被调用的时候后面可以跟任意数目的参数。
// 例如fmt.Println就是一个常用的可变参函数。

package main

import "fmt"

// Here's a function that will take an arbitrary number
// of `ints` as arguments.
// 这里是一个将任意数量的整形作为参数的函数。
func sum(nums ...int) {
	fmt.Print(nums, " ")
	total := 0
	for _, num := range nums {
		total += num
	}
	fmt.Println(total)
}

func main() {

	// Variadic functions can be called in the usual way
	// with individual arguments.
	// 可以用不同的参数按照通常的方式调用可变参函数。
	sum(1, 2)
	sum(1, 2, 3)

	// If you already have multiple args in a slice,
	// apply them to a variadic function using
	// `func(slice...)` like this.
	// 如果你已经有多个参数在切片中，
	// 可以用像func(slice...)的方式把他们传递给可变参函数。
	nums := []int{1, 2, 3, 4}
	sum(nums...)
}

// 输出
//[1 2] 3
//[1 2 3] 6
//[1 2 3 4] 10

```