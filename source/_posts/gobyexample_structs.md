title: GoByExample_Structs
date: 2014-03-04 20:58:45
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

**go 结构体**

<!--more-->

<!--body-->

``` go
// GoByExample_Structs project main.go

// Go's _structs_ are typed collections of fields.
// They're useful for grouping data together to form
// records.
// go语言结构体是一个字段集合类型。

package main

import "fmt"

// This `person` struct type has `name` and `age` fields.
// 这个person结构体类型有name和age字段。
type person struct {
	name string
	age  int
}

func main() {

	// This syntax creates a new struct.
	// 这个语法创建了一个新的结构体。
	fmt.Println(person{"Bob", 20})

	// You can name the fields when initializing a struct.
	// 当初始化结构体的时候你可以给字段命名。
	fmt.Println(person{name: "Alice", age: 30})

	// Omitted fields will be zero-valued.
	// 省缺的字段将会被其0值填充。
	fmt.Println(person{name: "Fred"})

	// An `&` prefix yields a pointer to the struct.
	// 这个开始的&符号将产生一个指针到结构体。
	fmt.Println(&person{name: "Ann", age: 40})

	// Access struct fields with a dot.
	// 用点来访问结构体的字段。
	s := person{name: "Sean", age: 50}
	fmt.Println(s.name)

	// You can also use dots with struct pointers - the
	// pointers are automatically dereferenced.
	// 你也可以用点和指针连用，指针会自动引用的。
	sp := &s
	fmt.Println(sp.age)

	// Structs are mutable.
	// 结构体是可变的。
	sp.age = 51
	fmt.Println(sp.age)
}

// 输出
//{Bob 20}
//{Alice 30}
//{Fred 0}
//&{Ann 40}
//Sean
//50
//51

```