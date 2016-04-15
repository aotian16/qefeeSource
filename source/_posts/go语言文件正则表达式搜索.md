title: go语言文件正则表达式搜索
date: 2014-02-05 19:48:40
categories: go
tags: go
---

<!--head-->

go语言遍历文件夹还是很方便的,一个walk就搞定了,在此基础上我试了试正则表达式查找功能.

**涉及的技术有**

1. 正则表达式
2. 文件夹遍历

代码如下:

<!--more-->

<!--body-->

```
package main

import (
	"fmt"
	"os"
	"path/filepath"
	"regexp"
)

func main() {
	// 命令行参数
	args := os.Args

	// 检查参数
	if len(args) == 1 {
		fmt.Println("ff is a file find tool. use like bottom")
		fmt.Println("ff [dir] [regexp]")
		return
	}

	if len(args) < 3 {
		fmt.Println("args < 3")
		return
	}

	fileName := args[1]
	pattern := args[2]

	file, err := os.Open(fileName)

	if err != nil {
		fmt.Println(err)
		return
	}

	fi, err := file.Stat()

	if err != nil {
		fmt.Println(err)
		return
	}

	if !fi.IsDir() {
		fmt.Println(fileName, " is not a dir")
	}

	reg, err := regexp.Compile(pattern)

	if err != nil {
		fmt.Println(err)
		return
	}

	// 遍历目录
	filepath.Walk(fileName,
		func(path string, f os.FileInfo, err error) error {
			if err != nil {
				fmt.Println(err)
				return err
			}

			if f.IsDir() {
				return nil
			}

			// 匹配目录
			matched := reg.MatchString(f.Name())

			if matched {
				fmt.Println(path)
			}

			return nil
		})

}
```