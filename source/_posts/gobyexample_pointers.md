title: GoByExample_Pointers
date: 2014-03-04 20:58:44
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

**go 指针**

<!--more-->

<!--body-->

``` go
// GoByExample_Pointers project main.go

// Go supports <em><a href="http://en.wikipedia.org/wiki/Pointer_(computer_programming)">pointers</a></em>,
// allowing you to pass references to values and records
// within your program.
// go语言支持指针，允许你用引用传递值或者记录。

package main

import "fmt"

// We'll show how pointers work in contrast to values with
// 2 functions: `zeroval` and `zeroptr`. `zeroval` has an
// `int` parameter, so arguments will be passed to it by
// value. `zeroval` will get a copy of `ival` distinct
// from the one in the calling function.
// 我们将通过2个函数：zeroval 和zeroptr对照来显示指针是怎么工作的。
// zeroval函数有一个整形的参数，所以参数是传值的方式。
// zeroval将获取到一个和调用时候的值不同的拷贝。
func zeroval(ival int) {
	ival = 0
}

// `zeroptr` in contrast has an `*int` parameter, meaning
// that it takes an `int` pointer. The `*iptr` code in the
// function body then _dereferences_ the pointer from its
// memory address to the current value at that address.
// Assigning a value to a dereferenced pointer changes the
// value at the referenced address.
// zeroptr相反，他有一个*int的参数，表示它是一个指针。
// 这*iptr在函数体中会引用指针指向的内存地址的值。
// 给指针赋值将改变指针指向的地址的值。
func zeroptr(iptr *int) {
	*iptr = 0
}

func main() {
	i := 1
	fmt.Println("initial:", i)

	zeroval(i)
	fmt.Println("zeroval:", i)

	// The `&i` syntax gives the memory address of `i`,
	// i.e. a pointer to `i`.
	// 语法&i将获取获取i的内存地址，即一个指向i的指针。
	zeroptr(&i)
	fmt.Println("zeroptr:", i)

	// Pointers can be printed too.
	// 指针也可以被打印。
	fmt.Println("pointer:", &i)

	// zeroval函数不能改变main函数中的i，
	// 但是zeroptr能是因为它有一个引用指向这个变量的内存地址。
}

// 输出
//initial: 1
//zeroval: 1
//zeroptr: 0
//pointer: 0x115c00e0

```