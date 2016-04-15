title: gobyexample_closing_channels
date: 2014-03-12 21:40:32
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

**go 关闭管道**

<!--more-->

<!--body-->

``` go
// GoByExample_ClosingChinnels project main.go

// _Closing_ a channel indicates that no more values
// will be sent on it. This can be useful to communicate
// completion to the channel's receivers.
// 关闭一个channel，表明没有值再发送了。
// 这可以用来告诉接受者channel已经完成。

package main

import "fmt"

// In this example we'll use a `jobs` channel to
// communicate work to be done from the `main()` goroutine
// to a worker goroutine. When we have no more jobs for
// the worker we'll `close` the `jobs` channel.
// 在这个例子中，我们在main main() goroutine将用一个job
// 这个channel来告诉worker job已经完了。
// 当没有更多job的给work时候，我们将关闭job。
func main() {
	jobs := make(chan int, 5)
	done := make(chan bool)

	// Here's the worker goroutine. It repeatedly receives
	// from `jobs` with `j, more := <-jobs`. In this
	// special 2-value form of receive, the `more` value
	// will be `false` if `jobs` has been `close`d and all
	// values in the channel have already been received.
	// We use this to notify on `done` when we've worked
	// all our jobs.
	// 这里有一个工作goroutine。他重复的用j, more := <-jobs从jobs接收。
	// 当channel关闭并且所有的值被接收后，变量more接收到的值是false。
	// 我们通过判断这个来给done这个channel通知我们完成了我们的job。
	go func() {
		for {
			j, more := <-jobs
			if more {
				fmt.Println("received job", j)
			} else {
				fmt.Println("received all jobs")
				done <- true
				return
			}
		}
	}()

	// This sends 3 jobs to the worker over the `jobs`
	// channel, then closes it.
	//  这里通过job这个channel发送了3个job给work。
	// 然后我们关闭了它。
	for j := 1; j <= 3; j++ {
		jobs <- j
		fmt.Println("sent job", j)
	}
	close(jobs)
	fmt.Println("sent all jobs")

	// We await the worker using the
	// [synchronization](channel-synchronization) approach
	// we saw earlier.
	// 我们用前面说的的同步方法来等待worker。
	<-done
}

// 输出
//sent job 1
//sent job 2
//sent job 3
//sent all jobs
//received job 1
//received job 2
//received job 3
//received all jobs

```