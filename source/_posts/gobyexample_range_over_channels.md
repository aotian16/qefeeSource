title: gobyexample_range_over_channels
date: 2014-03-13 19:45:11
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

**go 遍历管道**

<!--more-->

<!--body-->

``` go
// GoByExample_RangeOverChannels project main.go

// In a [previous](range) example we saw how `for` and
// `range` provide iteration over basic data structures.
// We can also use this syntax to iterate over
// values received from a channel.
// 在遍历的示例中range为基本的数据类型提供了迭代功能。
// 我们同样能用这个语法来迭代从channel接收的值。

package main

import "fmt"

func main() {

	// We'll iterate over 2 values in the `queue` channel.
	// 我们将从queue这个channel中遍历2个值。
	queue := make(chan string, 2)
	queue <- "one"
	queue <- "two"
	close(queue)

	// This `range` iterates over each element as it's
	// received from `queue`. Because we `close`d the
	// channel above, the iteration terminates after
	// receiving the 2 elements. If we didn't `close` it
	// we'd block on a 3rd receive in the loop.
	// 这个range遍历所有从queue接收的元素。
	// 由于我们在前面关闭了channel，
	// 迭代器会在接收2个元素后关闭，如果不，
	// 循环会被阻塞知道收到第三个元素
	//（经过我测试如果没有close的话，运行的时候会报错，
	// 程序自己会杀掉main的goroutine）。
	for elem := range queue {
		fmt.Println(elem)
	}
}

// 输出
//one
//two

```