title: gobyexample_ratelimiting
date: 2014-03-14 23:48:29
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

**go 工作池**

<!--more-->

<!--body-->

``` go
// GoByExample_RateLimiting project main.go

// _[Rate limiting](http://en.wikipedia.org/wiki/Rate_limiting)_
// is an important mechanism for controlling resource
// utilization and maintaining quality of service. Go
// elegantly supports rate limiting with goroutines,
// channels, and [tickers](tickers).
// 流量控制是控制资源利用率和保持服务质量的一个重要机制。
// go语言用goroutin，channel和 tickers优美的支持了流量控制。

package main

import "time"
import "fmt"

func main() {

	// First we'll look at basic rate limiting. Suppose
	// we want to limit our handling of incoming requests.
	// We'll serve these requests off a channel of the
	// same name.
	//  首先我们看看基本的流量控制。假设我们需要限制在处理的请求。
	// 我们会为这些请求的采用相同channel。
	requests := make(chan int, 5)
	for i := 1; i <= 5; i++ {
		requests <- i
	}
	close(requests)

	// This `limiter` channel will receive a value
	// every 200 milliseconds. This is the regulator in
	// our rate limiting scheme.
	// 这个限流的channel将会每200毫秒收到一个值。
	// 这是一个调节我们限制速率的一个方案。
	limiter := time.Tick(time.Millisecond * 200)

	// By blocking on a receive from the `limiter` channel
	// before serving each request, we limit ourselves to
	// 1 request every 200 milliseconds.
	// 在服务每个请求之前,阻塞`limiter`,以此,
	// 我们限制每200ms1个请求.
	for req := range requests {
		<-limiter
		fmt.Println("request", req, time.Now())
	}

	// We may want to allow short bursts of requests in
	// our rate limiting scheme while preserving the
	// overall rate limit. We can accomplish this by
	// buffering our limiter channel. This `burstyLimiter`
	// channel will allow bursts of up to 3 events.
	// 我们也许想有一个短小并发在限流里面，但是整体上是限流的。
	// 这可以通过有缓冲限制的channel来实现。
	// burstyLimiter channel 将只允许最多3个并发事件。
	burstyLimiter := make(chan time.Time, 3)

	// Fill up the channel to represent allowed bursting.
	// 填充表示允许并发的 channel。
	for i := 0; i < 3; i++ {
		burstyLimiter <- time.Now()
	}

	// Every 200 milliseconds we'll try to add a new
	// value to `burstyLimiter`, up to its limit of 3.
	// 每200毫秒我们试着添加新的值到burstyLimiter，最多3个。
	go func() {
		for t := range time.Tick(time.Millisecond * 200) {
			burstyLimiter <- t
		}
	}()

	// Now simulate 5 more incoming requests. The first
	// 3 of these will benefit from the burst capability
	// of `burstyLimiter`.
	// 现在模拟5个请求。前三个将受益于有并发能力的burstyLimiter。
	burstyRequests := make(chan int, 5)
	for i := 1; i <= 5; i++ {
		burstyRequests <- i
	}
	close(burstyRequests)
	for req := range burstyRequests {
		<-burstyLimiter
		fmt.Println("request", req, time.Now())
	}
}

// 输出
//request 1 2014-03-14 23:33:06.2729798 +0900 +0900
//request 2 2014-03-14 23:33:06.4731683 +0900 +0900
//request 3 2014-03-14 23:33:06.6728801 +0900 +0900
//request 4 2014-03-14 23:33:06.8732695 +0900 +0900
//request 5 2014-03-14 23:33:07.0735371 +0900 +0900
//request 1 2014-03-14 23:33:07.0735371 +0900 +0900
//request 2 2014-03-14 23:33:07.0735371 +0900 +0900
//request 3 2014-03-14 23:33:07.0735371 +0900 +0900
//request 4 2014-03-14 23:33:07.2739818 +0900 +0900
//request 5 2014-03-14 23:33:07.4751902 +0900 +0900

```