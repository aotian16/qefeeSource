title: gobyexample_sorting
date: 2014-03-16 21:24:55
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

**go 排序**

<!--more-->

<!--body-->

``` go
// GoByExample_Sorting project main.go

// Go's `sort` package implements sorting for builtins
// and user-defined types. We'll look at sorting for
// builtins first.
// go语言的sort包实现了对内置数据类型和自定义数据类型的排序功能。
// 首先我们看对内置数据类型的排序。

package main

import "fmt"
import "sort"

func main() {

	// Sort methods are specific to the builtin type;
	// here's an example for strings. Note that sorting is
	// in-place, so it changes the given slice and doesn't
	// return a new one.
	// 内置类型都有特定的排序方法。这里是一个string类型的示例。
	// 注意排序是就地排序，所以会改变传入的切片，不返回一个新切片。
	strs := []string{"c", "a", "b"}
	sort.Strings(strs)
	fmt.Println("Strings:", strs)

	// An example of sorting `int`s.
	// 一个对int型排序的示例。
	ints := []int{7, 2, 4}
	sort.Ints(ints)
	fmt.Println("Ints:   ", ints)

	// We can also use `sort` to check if a slice is
	// already in sorted order.
	// 我们也可以用sort包来检查一个切片是否是排序好的。
	s := sort.IntsAreSorted(ints)
	fmt.Println("Sorted: ", s)
}

// 输出
//Ints:    [2 4 7]
//Sorted:  true

```