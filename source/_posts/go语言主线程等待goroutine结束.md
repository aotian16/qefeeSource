title: go语言主线程等待goroutine结束
date: 2014-02-04 19:27:08
categories: go
tags: go
---

<!--head-->

在go语言中如何让主线程等待goroutine结束呢?
主要用到了

**sync.WaitGroup**

直接看代码吧

<!--more-->

<!--body-->

```
// TestWaitGroup project main.go
package main

import (
	"fmt"
	"sync"
	"time"
)

func main() {
	// 定义WaitGroup
	var wg sync.WaitGroup

	// 计数器+1
	wg.Add(1)
	go foo(&wg, "a")

	time.Sleep(time.Second * 3)

	// 计数器+1
	wg.Add(1)
	go foo(&wg, "b")

	fmt.Println("before wait")

	// 等待WaitGroup计数器归零
	wg.Wait()

	fmt.Println("after wait")
}

func foo(wg *sync.WaitGroup, name string) {
	for i := 0; i < 10; i++ {
		fmt.Println(name, i)
		time.Sleep(time.Second)
	}
	// 计数器-1
	wg.Done()
}
```