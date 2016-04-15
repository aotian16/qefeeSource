title: gobyexample_workerpools
date: 2014-03-14 23:12:56
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

**go 流量限制**

<!--more-->

<!--body-->

``` go
// GoByExample_WorkerPools project main.go

// In this example we'll look at how to implement
// a _worker pool_ using goroutines and channels.
// 在这个示例中为我们将会看到怎么用goroutines和
// channels去实现一个工作池

package main

import "fmt"
import "time"

// Here's the worker, of which we'll run several
// concurrent instances. These workers will receive
// work on the `jobs` channel and send the corresponding
// results on `results`. We'll sleep a second per job to
// simulate an expensive task.
// 这是哪个工作函数，我们将运行它的多个并发实例。
// 这些工作实例将接收从jobs这个channel来的值，
// 然后发送结果到对应的results channel中。
// 我们将在每个任务停1秒以用来模拟一个昂贵的任务。
func worker(id int, jobs <-chan int, results chan<- int) {
	for j := range jobs {
		fmt.Println("worker", id, "processing job", j)
		time.Sleep(time.Second)
		results <- j * 2
	}
}

func main() {

	// In order to use our pool of workers we need to send
	// them work and collect their results. We make 2
	// channels for this.
	//  为了使用工作池，我们需要给工作池发送任务同时获取他们的结果。
	// 我们创建了两个channel（jobs和results）来做这个。
	jobs := make(chan int, 100)
	results := make(chan int, 100)

	// This starts up 3 workers, initially blocked
	// because there are no jobs yet.
	// 这里开了3个工作实例，他们将被阻塞，因为还没有任务进来。
	for w := 1; w <= 3; w++ {
		go worker(w, jobs, results)
	}

	// Here we send 9 `jobs` and then `close` that
	// channel to indicate that's all the work we have.
	// 这里我们发送9个"任务"然后关闭了管道,
	// 表明这是我们所有的任务
	for j := 1; j <= 9; j++ {
		jobs <- j
	}
	close(jobs)

	// Finally we collect all the results of the work.
	// 最后我们们要获取所有的工作结果。
	for a := 1; a <= 9; a++ {
		<-results
	}
}

// 输出
//worker 1 processing job 1
//worker 2 processing job 2
//worker 3 processing job 3
//worker 1 processing job 4
//worker 3 processing job 5
//worker 2 processing job 6
//worker 1 processing job 7
//worker 2 processing job 8
//worker 3 processing job 9

```