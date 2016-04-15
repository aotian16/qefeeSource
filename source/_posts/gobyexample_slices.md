title: GoByExample_Slices
date: 2014-03-04 20:58:36
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

**go 切片**

<!--more-->

<!--body-->

``` go
// GoByExample_Slices project main.go

// _Slices_ are a key data type in Go, giving a more
// powerful interface to sequences than arrays.
// 在go语言里slices是一个重要的数据类型，
// 它为数组访问提供了更强的接口。

package main

import "fmt"

func main() {

	// Unlike arrays, slices are typed only by the
	// elements they contain (not the number of elements).
	// To create an empty slice with non-zero length, use
	// the builtin `make`. Here we make a slice of
	// `string`s of length `3` (initially zero-valued).
	// 和数组不同的是，切片的类型只由其包含的元素类型来决定，
	// 而不是由元素个数和元素类型共同决定。
	// 内置函数make，	用来创建一个非0长度的空切片。
	// 这里我们创建了一个长度为3的字符串切片（初始化化为对应的0值）。
	s := make([]string, 3)
	fmt.Println("emp:", s)

	// We can set and get just like with arrays.
	// 我们能像数组一样来设置和获取切片元素的值。
	s[0] = "a"
	s[1] = "b"
	s[2] = "c"
	fmt.Println("set:", s)
	fmt.Println("get:", s[2])

	// `len` returns the length of the slice as expected.
	// 和我们预期的一样，同样可以用len函数来获取切片的长度。
	fmt.Println("len:", len(s))

	// In addition to these basic operations, slices
	// support several more that make them richer than
	// arrays. One is the builtin `append`, which
	// returns a slice containing one or more new values.
	// Note that we need to accept a return value from
	// append as we may get a new slice value.
	// 切片比数组功能更强大了很多。
	// 其中一个就是内置函数append,它返回一个增加一个或多个值的新切片。
	// 注意，我们必须去接收这个返回的新切片。
	s = append(s, "d")
	s = append(s, "e", "f")
	fmt.Println("apd:", s)

	// Slices can also be `copy`'d. Here we create an
	// empty slice `c` of the same length as `s` and copy
	// into `c` from `s`.
	// 切片同样也能被复制，这里我们创建了一个和s切片有同样长度的切片c，
	// 然后我们从s切片复制了值到c切片。
	c := make([]string, len(s))
	copy(c, s)
	fmt.Println("cpy:", c)

	// Slices support a "slice" operator with the syntax
	// `slice[low:high]`. For example, this gets a slice
	// of the elements `s[2]`, `s[3]`, and `s[4]`.
	// 通过语法slice[low:high]，切片类型支持“切片”的操作。
	// 例如，这里（l := s[2:5]）将会获取到元素s[2], s[3], 和s[4]。
	l := s[2:5]
	fmt.Println("sl1:", l)

	// This slices up to (but excluding) `s[5]`.
	// 这个切片到达s[5],但是不包含s[5]。
	l = s[:5]
	fmt.Println("sl2:", l)

	// And this slices up from (and including) `s[2]`.
	// 这个切片开始于s[2],且包含s[2]。
	l = s[2:]
	fmt.Println("sl3:", l)

	// We can declare and initialize a variable for slice
	// in a single line as well.
	t := []string{"g", "h", "i"}
	fmt.Println("dcl:", t)

	// Slices can be composed into multi-dimensional data
	// structures. The length of the inner slices can
	// vary, unlike with multi-dimensional arrays.
	// 切片也可以组成复杂的多维切片。
	// 和多位数组不同的是，内层切片的长度是可变的。
	twoD := make([][]int, 3)
	for i := 0; i < 3; i++ {
		innerLen := i + 1
		twoD[i] = make([]int, innerLen)
		for j := 0; j < innerLen; j++ {
			twoD[i][j] = i + j
		}
	}
	fmt.Println("2d: ", twoD)
}

// 输出
//emp: [  ]
//set: [a b c]
//get: c
//len: 3
//apd: [a b c d e f]
//cpy: [a b c d e f]
//sl1: [c d e]
//sl2: [a b c d e]
//sl3: [c d e f]
//dcl: [g h i]
//2d:  [[0] [1 2] [2 3 4]]

```