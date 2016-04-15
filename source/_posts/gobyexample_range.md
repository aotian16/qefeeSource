title: GoByExample_Range
date: 2014-03-04 20:58:38
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

**go range范围**

<!--more-->

<!--body-->

``` go
// GoByExample_Range project main.go

// _range_ iterates over of elements in a variety of
// data structures. Let's see how to use `range` with some
// of the data structures we've already learned.
// range遍历各种数据中的元素。
// 让我们看看怎么来遍历一些我们已经学习过的数据结构。

package main

import "fmt"

func main() {

	// Here we use `range` to sum the numbers in a slice.
	// Arrays work like this too.
	// 这里我们通过range来遍历切片来求和。
	// 数组也和这一样。
	nums := []int{2, 3, 4}
	sum := 0
	for _, num := range nums {
		sum += num
	}
	fmt.Println("sum:", sum)

	// `range` on arrays and slices provides both the
	// index and value for each entry. Above we didn't
	// need the index, so we ignored it with the
	// _blank identifier_ `_`. Sometimes we actually want
	// the indexes though.
	// range在数组和切片上的时候可以提供每个元素的索引和值。
	// 在上面我们不需要索引，所以我们也能够用`_`字符忽略索引值。
	// 但是有时候我们的确会很需要它的。
	for i, num := range nums {
		if num == 3 {
			fmt.Println("index:", i)
		}
	}

	// `range` on map iterates over key/value pairs.
	// 用range遍历map的key/value。
	kvs := map[string]string{"a": "apple", "b": "banana"}
	for k, v := range kvs {
		fmt.Printf("%s -> %s\n", k, v)
	}

	// `range` on strings iterates over Unicode code
	// points. The first value is the starting byte index
	// of the `rune` and the second the `rune` itself.
	// 当range应用于字符串的时候，遍历的时其unicode 编码指针。
	// 第一个值是字符的字节索引值，第二个是uniocede编码值。
	// *注意:由于中文占3个字节，序号需要注意
	for i, c := range "go我试试中文" {
		fmt.Println(i, c)
	}
}

// 输出
//sum: 9
//index: 1
//a -> apple
//b -> banana
//0 103
//1 111
//2 25105
//5 35797
//8 35797
//11 20013
//14 25991

```