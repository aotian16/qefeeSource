title: GoByExample_Arrays
date: 2014-03-04 20:58:35
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

**go 数组**

<!--more-->

<!--body-->

``` go
// GoByExample_Arrays project main.go

// In Go, an _array_ is a numbered sequence of elements of a
// specific length.
// 在go语言中，数组是一个有编号的元素序列，它具有特定长度。

package main

import "fmt"

func main() {

	// Here we create an array `a` that will hold exactly
	// 5 `int`s. The type of elements and length are both
	// part of the array's type. By default an array is
	// zero-valued, which for `int`s means `0`s.
	// 这里我们我们创建了一个5个int型的数组。 数组的长度和元素的类型，
	// 都是数组的类型。
	var a [5]int
	fmt.Println("emp:", a)

	// We can set a value at an index using the
	// `array[index] = value` syntax, and get a value with
	// `array[index]`.
	// 在int型数组中其元素的默认值是0.我们可以通过
	// array[index] = value 语句设置数组元素的值，
	// 通过array[index]语句获取值。
	a[4] = 100
	fmt.Println("set:", a)
	fmt.Println("get:", a[4])

	// The builtin `len` returns the length of an array.
	// 内建的len函数能够获取到数组的长度。
	fmt.Println("len:", len(a))

	// Use this syntax to declare and initialize an array
	// in one line.
	// 通过以下语句形式，在一行中声明和初始化初始化数组。
	b := [5]int{1, 2, 3, 4, 5}
	fmt.Println("dcl:", b)

	// Array types are one-dimensional, but you can
	// compose types to build multi-dimensional data
	// structures.
	// 数组类型是一维，但您可以组成类型建立多维数据结构
	var twoD [2][3]int
	for i := 0; i < 2; i++ {
		for j := 0; j < 3; j++ {
			twoD[i][j] = i + j
		}
	}
	fmt.Println("2d: ", twoD)
}

// 输出
//emp: [0 0 0 0 0]
//set: [0 0 0 0 100]
//get: 100
//len: 5
//dcl: [1 2 3 4 5]
//2d:  [[0 1 2] [1 2 3]]

```