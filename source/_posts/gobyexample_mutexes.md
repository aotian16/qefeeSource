title: gobyexample_mutexes
date: 2014-03-15 21:18:45
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

**go 互斥**

<!--more-->

<!--body-->

``` go
// GoByExample_Mutexes project main.go

// In the previous example we saw how to manage simple
// counter state using atomic operations. For more complex
// state we can use a _[mutex](http://en.wikipedia.org/wiki/Mutual_exclusion)_
// to safely access data across multiple goroutines.
// 在前面的示例中我们看到用原子操作来管理一个简单的计数器的状态。
// 对于更复杂的状态，我们可以使用一个互斥锁来安全地访问多个goroutines的数据。

package main

import (
	"fmt"
	"math/rand"
	"runtime"
	"sync"
	"sync/atomic"
	"time"
)

func main() {

	// For our example the `state` will be a map.
	// 在我们的示例中这个状态是一个map。
	var state = make(map[int]int)

	// This `mutex` will synchronize access to `state`.
	// 这个互斥锁将同步状态。
	var mutex = &sync.Mutex{}

	// To compare the mutex-based approach with another
	// we'll see later, `ops` will count how many
	// operations we perform against the state.
	// 为了和后面我们将看到基于互斥锁的方法进行比较，
	// ops变量将统计我们操作了状态多少次。
	var ops int64 = 0

	// Here we start 100 goroutines to execute repeated
	// reads against the state.
	// 我们开100 goroutines 不停的去读取状态。
	for r := 0; r < 100; r++ {
		go func() {
			total := 0
			for {

				// For each read we pick a key to access,
				// `Lock()` the `mutex` to ensure
				// exclusive access to the `state`, read
				// the value at the chosen key,
				// `Unlock()` the mutex, and increment
				// the `ops` count.
				// 在每一次读取时，我们先选择一个key，
				// 然后用Lock()锁定确保独立的访问，
				// 并通过key来读取状态，最后Unlock()，
				// 并增加计数器ops。
				key := rand.Intn(5)
				mutex.Lock()
				total += state[key]
				mutex.Unlock()
				atomic.AddInt64(&ops, 1)

				// In order to ensure that this goroutine
				// doesn't starve the scheduler, we explicitly
				// yield after each operation with
				// `runtime.Gosched()`. This yielding is
				// handled automatically with e.g. every
				// channel operation and for blocking
				// calls like `time.Sleep`, but in this
				// case we need to do it manually.
				//  为了确保goroutine 让出调度时间，
				// 我们必须在每次操作后明确调用runtime.Gosched()。
				// 本来这个是会自动处理的，但是这里我们必须自己手动调用。
				runtime.Gosched()
			}
		}()
	}

	// We'll also start 10 goroutines to simulate writes,
	// using the same pattern we did for reads.
	// 我们还将启动10个goroutine来模拟写入，使用和读取相同的模式。
	for w := 0; w < 10; w++ {
		go func() {
			for {
				key := rand.Intn(5)
				val := rand.Intn(100)
				mutex.Lock()
				state[key] = val
				mutex.Unlock()
				atomic.AddInt64(&ops, 1)
				runtime.Gosched()
			}
		}()
	}

	// Let the 10 goroutines work on the `state` and
	// `mutex` for a second.
	// 让这10个goroutine在这种情况下工作1秒钟。
	time.Sleep(time.Second)

	// Take and report a final operations count.
	// 采集和报告最终得计数结果。
	opsFinal := atomic.LoadInt64(&ops)
	fmt.Println("ops:", opsFinal)

	// With a final lock of `state`, show how it ended up.
	// 最后锁定，显示一下最终状态。
	mutex.Lock()
	fmt.Println("state:", state)
	mutex.Unlock()
}

// 输出
//ops: 5031532
//state: map[4:3 2:80 1:59 0:35 3:19]

```