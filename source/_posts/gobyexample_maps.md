title: GoByExample_Maps
date: 2014-03-04 20:58:37
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

**go map字典**

<!--more-->

<!--body-->

``` go
// GoByExample_Maps project main.go

// _Maps_ are Go's built-in [associative data type](http://en.wikipedia.org/wiki/Associative_array)
// (sometimes called _hashes_ or _dicts_ in other languages).
// Maps 是一种数据映射结构（有时候我们也叫hash类型或者字典类型在别的语言里）。

package main

import "fmt"

func main() {

	// To create an empty map, use the builtin `make`:
	// `make(map[key-type]val-type)`.
	// 为了创建一个空map结构，
	// 我们用内置的make函数：make(map[key-type]val-type)。
	m := make(map[string]int)

	// Set key/value pairs using typical `name[key] = val`
	// syntax.
	// 设置典型的 key/value 对用典型的name[key] = val语句。
	m["k1"] = 7
	m["k2"] = 13

	// Printing a map with e.g. `Println` will show all of
	// its key/value pairs.
	// 打印map的例子用Println语句将展现key/value对。
	fmt.Println("map:", m)

	// Get a value for a key with `name[key]`.
	// 获取value值用语句name[key]。
	v1 := m["k1"]
	fmt.Println("v1: ", v1)

	// The builtin `len` returns the number of key/value
	// pairs when called on a map.
	// 对map使用内置的len函数将返回key/value对的个数。
	fmt.Println("len:", len(m))

	// The builtin `delete` removes key/value pairs from
	// a map.
	// 内置的delete函数可以从map中移除key/value对。
	delete(m, "k2")
	fmt.Println("map:", m)

	// The optional second return value when getting a
	// value from a map indicates if the key was present
	// in the map. This can be used to disambiguate
	// between missing keys and keys with zero values
	// like `0` or `""`.
	// 当从map获取值时候，可选的第二个返回值能够标识key在map中是否存在。
	// 这可以用来消除缺少键和键与像 0 的零值或""之间的歧义。
	_, prs := m["k2"]
	fmt.Println("prs:", prs)

	// You can also declare and initialize a new map in
	// the same line with this syntax.
	// 你同样也能声明和初始化一个新的map在一条语句中
	n := map[string]int{"foo": 1, "bar": 2}
	fmt.Println("map:", n)
}

// 输出
//map: map[k1:7 k2:13]
//v1:  7
//len: 2
//map: map[k1:7]
//prs: false
//map: map[foo:1 bar:2]

```