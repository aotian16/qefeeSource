title: gobyexample_errors
date: 2014-03-07 23:43:40
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

**go 错误**

<!--more-->

<!--body-->

``` go
// GoByExample_Errors project main.go

// In Go it's idiomatic to communicate errors via an
// explicit, separate return value. This contrasts with
// the exceptions used in languages like Java and Ruby and
// the overloaded single result / error value sometimes
// used in C. Go's approach makes it easy to see which
// functions return errors and to handle them using the
// same language constructs employed for any other,
// non-error tasks.

// 在go语言中，错误是地道的显式单独返回的。
// 和那些用异常机制的（例如java和ruby）
// 或者有时候重载错误/结果的（例如c）语言正好相反。
// go语言的方式可以使得我们可以轻松的去看到哪些函数返回了错误，
// 并用相同的语言结构去处理错误和其他非错误任务。

package main

import "errors"
import "fmt"

// By convention, errors are the last return value and
// have type `error`, a built-in interface.
// 按照惯例，错误是最后的返回值，
// 它有一个内置的interface类型：error类型。
func f1(arg int) (int, error) {
	if arg == 42 {

		// `errors.New` constructs a basic `error` value
		// with the given error message.
		// errors.New通过给定的错误消息，构建了一个基本的错误值。
		return -1, errors.New("can't work with 42")

	}

	// A nil value in the error position indicates that
	// there was no error.
	// 一个nil值在error的位置表明这里没有错误。
	return arg + 3, nil
}

// It's possible to use custom types as `error`s by
// implementing the `Error()` method on them. Here's a
// variant on the example above that uses a custom type
// to explicitly represent an argument error.
// 也可以通过实现Error方法来定义自己的错误类型。
type argError struct {
	arg  int
	prob string
}

func (e *argError) Error() string {
	return fmt.Sprintf("%d - %s", e.arg, e.prob)
}

func f2(arg int) (int, error) {
	if arg == 42 {

		// In this case we use `&argError` syntax to build
		// a new struct, supplying values for the two
		// fields `arg` and `prob`.
		// 在这个示例中，我们通过&argError语法定义了一个新的结构体，
		// 有两个字段arg和prob.
		return -1, &argError{arg, "can't work with it"}
	}
	return arg + 3, nil
}

func main() {

	// The two loops below test out each of our
	// error-returning functions. Note that the use of an
	// inline error check on the `if` line is a common
	// idiom in Go code.
	// 下面的两个循环测试每个函数的错误返回功能。
	// 请注意在go代码中，通常在if语句中使用内嵌错误机制检查代码.
	for _, i := range []int{7, 42} {
		if r, e := f1(i); e != nil {
			fmt.Println("f1 failed:", e)
		} else {
			fmt.Println("f1 worked:", r)
		}
	}
	for _, i := range []int{7, 42} {
		if r, e := f2(i); e != nil {
			fmt.Println("f2 failed:", e)
		} else {
			fmt.Println("f2 worked:", r)
		}
	}

	// If you want to programmatically use the data in
	// a custom error, you'll need to get the error  as an
	// instance of the custom error type via type
	// assertion.
	// 如果你需要通过自己定义的错误类型编程，
	// 你需要确定你获得的是你自己定义的错误类型。
	_, e := f2(42)
	if ae, ok := e.(*argError); ok {
		fmt.Println(ae.arg)
		fmt.Println(ae.prob)
	}
}

```