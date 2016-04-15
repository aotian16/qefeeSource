title: GoByExample_Methods
date: 2014-03-04 20:58:57
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

**go 方法**

<!--more-->

<!--body-->

``` go
// GoByExample_Methods project main.go

// Go supports _methods_ defined on struct types.
// go语言支持给结构体定义方法。

package main

import "fmt"

// 矩形结构体
type rect struct {
	width, height int
}

// This `area` method has a _receiver type_ of `*rect`.
// `area`方法有个类型为*rect的接收者。
// *这表明方法的执行对象
func (r *rect) area() int {
	return r.width * r.height
}

// Methods can be defined for either pointer or value
// receiver types. Here's an example of a value receiver.
// 方法即可以接受值类型，又可以接受值类型。这里有一个接收值类型的例子。
func (r rect) perim() int {
	return 2*r.width + 2*r.height
}

func main() {
	r := rect{width: 10, height: 5}

	// Here we call the 2 methods defined for our struct.
	// 这里我们用结构体调用以上定义的两个方法。
	fmt.Println("area: ", r.area())
	fmt.Println("perim:", r.perim())

	// Go automatically handles conversion between values
	// and pointers for method calls. You may want to use
	// a pointer receiver type to avoid copying on method
	// calls or to allow the method to mutate the
	// receiving struct.
	// go语言会自动处理方法调用的时候值和指针的转换。
	// 你可能会想用指针接收,用于避免在方法调用时候的复制，或改变结构体。
	rp := &r
	fmt.Println("area: ", rp.area())
	fmt.Println("perim:", rp.perim())
}

// 输出
//area:  50
//perim: 30
//area:  50
//perim: 30

```