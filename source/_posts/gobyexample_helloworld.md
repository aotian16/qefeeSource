title: GoByExample_HelloWorld
date: 2014-03-03 22:52:58
tags: gobyexample
categories: go
---

<!--head-->

想系统的过一遍go基础,看到这个教程.
**地址 - [gobyexample](https://gobyexample.com/ "gobyexample")**

还有一个这个教程的翻译帖子,(未全部完成).
**地址 - [go语言简单示例](http://bbs.csdn.net/topics/390557446 "go语言简单示例")**

多谢作者以及翻译人员,也欢迎光临我的小站
**地址 - [启飞](http://qefee.com "qefee")**

我做些什么呢,恩,就加些注释好了,O(∩_∩)O~

<!--more-->

<!--body-->

``` go
// GoByExample_HelloWorld project main.go

// 第一个程序总是从hello,world开始

// 包名
package main

// 导入包
import (
	"fmt" // 格式化输出包
)

// main函数
func main() {
	fmt.Println("Hello World!") // 打印hello,注意语句没有分号
}
```

**輸出:**

Hello World!