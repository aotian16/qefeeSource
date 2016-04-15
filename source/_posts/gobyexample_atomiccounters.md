title: gobyexample_atomiccounters
date: 2014-03-15 21:01:03
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

**go 原子计数器**

<!--more-->

<!--body-->

``` go
// GoByExample_AtomicCounters project main.go

// The primary mechanism for managing state in Go is
// communication over channels. We saw this for example
// with [worker pools](worker-pools). There are a few other
// options for managing state though. Here we'll
// look at using the `sync/atomic` package for _atomic
// counters_ accessed by multiple goroutines.
// 在go语言中，管理状态的主要机制是通过channle通讯。
// 这我们在前面的工作池里面已经看到过了。
// 但是还有一些其他的管理状态的方法。
// 这里我们将看到用 sync/atomic 包在多个goroutine访问原子计数器。

package main

import "fmt"
import "time"
import "sync/atomic"
import "runtime"

func main() {

	// We'll use an unsigned integer to represent our
	// (always-positive) counter.
	// 我们用无符号的整形来表示我们的计数器（总是正数）.
	var ops uint64 = 0

	// To simulate concurrent updates, we'll start 50
	// goroutines that each increment the counter about
	// once a millisecond.
	// 我们开启了50个goroutine，他们都将每毫秒钟更新一次计数器，
	// 用来模拟并发更新。
	for i := 0; i < 50; i++ {
		go func() {
			for {
				// To atomically increment the counter we
				// use `AddUint64`, giving it the memory
				// address of our `ops` counter with the
				// `&` syntax.
				// 我们用方法AddUint64来原子的增加计数器，
				// 同时用&语法来获取它的内存地址。
				atomic.AddUint64(&ops, 1)

				// Allow other goroutines to proceed.
				// 允许别的goroutine继续执行。
				runtime.Gosched()
			}
		}()
	}

	// Wait a second to allow some ops to accumulate.
	//  等待1秒来让计数器累加。
	time.Sleep(time.Second)

	// In order to safely use the counter while it's still
	// being updated by other goroutines, we extract a
	// copy of the current value into `opsFinal` via
	// `LoadUint64`. As above we need to give this
	// function the memory address `&ops` from which to
	// fetch the value.
	// 为了安全的使用还在被goroutine更新的计数器，
	// 我们通过方法LoadUint64提取了一个计数器的值的拷贝到变量opsFinal。
	// 正如上面一样，如果我们要提取这个值，我们需要用&ops给这个函数传递内存地址。
	opsFinal := atomic.LoadUint64(&ops)
	fmt.Println("ops:", opsFinal)
}

// 输出(根据电脑性能不同)
//ops: 10038007

```