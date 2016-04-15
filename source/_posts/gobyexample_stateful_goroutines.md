title: gobyexample_stateful_goroutines
date: 2014-03-16 21:19:36
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

**go 有状态的goroutine**

<!--more-->

<!--body-->

``` go
// GoByExample_StatefulGoroutines project main.go

// In the previous example we used explicit locking with
// mutexes to synchronize access to shared state across
// multiple goroutines. Another option is to use the
// built-in synchronization features of  goroutines and
// channels to achieve the same result. This channel-based
// approach aligns with Go's ideas of sharing memory by
// communicating and having each piece of data owned
// by exactly 1 goroutine.
// 在前面得例子中，我们显示的用锁定在多个goroutine中同步互斥锁中的状态。
// 另外的一个选择是用内置的同步功能来达到同样的效果。
// 这种基于channel的办法是用一个确切的goroutine的共享内存来交换
// 和管理每一块数据。

package main

import (
	"fmt"
	"math/rand"
	"sync/atomic"
	"time"
)

// In this example our state will be owned by a single
// goroutine. This will guarantee that the data is never
// corrupted with concurrent access. In order to read or
// write that state, other goroutines will send messages
// to the owning goroutine and receive corresponding
// replies. These `readOp` and `writeOp` `struct`s
// encapsulate those requests and a way for the owning
// goroutine to respond.
// 在这个示例中，我们的状态数据都在一个 goroutine中。
// 这将保证这些数据不被并发修改。为了读写这些状态数据，
// 别的goroutin将发送消息给拥有数据的goroutine并接收相应的回应。
// 结构体readOp和writeOp封装了这些请求并使拥有他们的goroutine回应的一个途径。
type readOp struct {
	key  int
	resp chan int
}
type writeOp struct {
	key  int
	val  int
	resp chan bool
}

func main() {

	// As before we'll count how many operations we perform.
	// 和以前一样，我们会统计多少操作被执行。
	var ops int64 = 0

	// The `reads` and `writes` channels will be used by
	// other goroutines to issue read and write requests,
	// respectively.
	// channel reads 和writes被分别用来发送读或写请求。
	reads := make(chan *readOp)
	writes := make(chan *writeOp)

	// Here is the goroutine that owns the `state`, which
	// is a map as in the previous example but now private
	// to the stateful goroutine. This goroutine repeatedly
	// selects on the `reads` and `writes` channels,
	// responding to requests as they arrive. A response
	// is executed by first performing the requested
	// operation and then sending a value on the response
	// channel `resp` to indicate success (and the desired
	// value in the case of `reads`).
	// 这就是那个拥有状态数据的goroutine。
	// 这个状态和前面的例子一样也是个map。
	// 但是在这里不一样的是他为保存状态的goroutine所私有的。
	// 这个goroutine不断的select reads和 writes这channel并在
	// 有请求到达后回应他们。一个响应时，首先执行请求的操作，
	// 然后给channel发送RESP值（和在读的情况下所需的值）来表示成功。
	go func() {
		var state = make(map[int]int)
		for {
			select {
			case read := <-reads:
				read.resp <- state[read.key]
			case write := <-writes:
				state[write.key] = write.val
				write.resp <- true
			}
		}
	}()

	// This starts 100 goroutines to issue reads to the
	// state-owning goroutine via the `reads` channel.
	// Each read requires constructing a `readOp`, sending
	// it over the `reads` channel, and the receiving the
	// result over the provided `resp` channel.
	// 这里将开启100个goroutine来通过reads channel发送读请求，
	// 每个读请求包含一个readOp结构体，并接收产生的相应的resp。
	for r := 0; r < 100; r++ {
		go func() {
			for {
				read := &readOp{
					key:  rand.Intn(5),
					resp: make(chan int)}
				reads <- read
				<-read.resp
				atomic.AddInt64(&ops, 1)
			}
		}()
	}

	// We start 10 writes as well, using a similar
	// approach.
	// 用同样的方式我们开启了10个写的goroutine。
	for w := 0; w < 10; w++ {
		go func() {
			for {
				write := &writeOp{
					key:  rand.Intn(5),
					val:  rand.Intn(100),
					resp: make(chan bool)}
				writes <- write
				<-write.resp
				atomic.AddInt64(&ops, 1)
			}
		}()
	}

	// Let the goroutines work for a second.
	// 让goroutine工作1s。
	time.Sleep(time.Second)

	// Finally, capture and report the `ops` count.
	// 最后获取和输出操作统计数。
	opsFinal := atomic.LoadInt64(&ops)
	fmt.Println("ops:", opsFinal)
}

// 输出
//ops: 1128091

```