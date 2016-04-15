title: go遍历文件
date: 2014-02-17 20:45:25
tags: go
categories: go
---

<!--head-->

遍历文件是一个很常见的操作, 虽然简单, 这里也记录下.

实现的方法有很多种,这里只试了scanner

**代码如下:**

<!--more-->

<!--body-->
```go
package main

import (
	"bufio"
	"fmt"
	"log"
	"os"
)

func main() {
	// 打开文件
	file, err := os.Open("C:\\temp\\file.txt")

	if err != nil {
		log.Fatal(err)
	}

	// 延迟关闭文件
	defer file.Close()

	// 遍历者
	scanner := bufio.NewScanner(file)

	// 开始遍历文件
	for scanner.Scan() {
		fmt.Println(scanner.Text()) // Println will add back the final '\n'
	}

	// 确认遍历中是否出错
	if scanner.Err() != nil {
		log.Fatal(scanner.Err())
	}
}
```