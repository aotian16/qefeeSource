title: gobyexample_interfaces
date: 2014-03-07 23:28:46
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

**go 接口**

<!--more-->

<!--body-->

``` go
// GoByExample_Interfaces project main.go

// _Interfaces_ are named collections of method
// signatures.
// 接口用来命名一些方法签名的集合.

package main

import "fmt"
import "math"

// Here's a basic interface for geometric shapes.
// 这是一个基本的图形接口.
type geometry interface {
	area() float64
	perim() float64
}

// For our example we'll implement this interface on
// `square` and `circle` types.
// 在我们的示例中我们将在方形和圆形类型中实现这个接口。
type square struct {
	width, height float64
}
type circle struct {
	radius float64
}

// To implement an interface in Go, we just need to
// implement all the methods in the interface. Here we
// implement `geometry` on `square`s.
// 为了在go语言中实现接口，我们只需要实现接口中的所有方法。
// 这里我在方形中实现了图形接口。
func (s square) area() float64 {
	return s.width * s.height
}
func (s square) perim() float64 {
	return 2*s.width + 2*s.height
}

// The implementation for `circle`s.
// 圆形的实现。
func (c circle) area() float64 {
	return math.Pi * c.radius * c.radius
}
func (c circle) perim() float64 {
	return 2 * math.Pi * c.radius
}

// If a variable has an interface type, then we can call
// methods that are in the named interface. Here's a
// generic `measure` function taking advantage of this
// to work on any `geometry`.
// 如果一个变量有一个接口类型，然后我们就能调用这个接口下的方法。
// 这里是一个通用的测量功能，利用这种方式可以对任何图形做测量。
func measure(g geometry) {
	fmt.Println(g)
	fmt.Println(g.area())
	fmt.Println(g.perim())
}

func main() {
	s := square{width: 3, height: 4}
	c := circle{radius: 5}

	// The `circle` and `square` struct types both
	// implement the `geometry` interface so we can use
	// instances of
	// these structs as arguments to `measure.
	// 圆形和方形结构体类型都实现了几何接口，
	// 这样我们就可以使用这些结构体的实例作为参数，
	// 并在函数内调用其测量功能。
	measure(s)
	measure(c)
}

```