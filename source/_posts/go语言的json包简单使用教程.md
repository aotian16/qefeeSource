title: go语言的json包简单使用教程
date: 2014-02-04 19:33:12
categories: go
tags: go
---

<!--head-->

主要看json和结构体的相互转换

直接看代码

<!--more-->

<!--body-->

```
// TestJSON project main.go
package main

import (
	"encoding/json"
	"fmt"
	"io"
	"os"
	"strings"
)

type Message struct {
	Id   int
	Name string
}

type ColorGroup struct {
	Id     int
	Name   string
	Colors []string
}

func main() {
	jsonToStruct()
	structToJson()
}

// json转结构体
func jsonToStruct() {
	const jsonStream = `
		{"Id": 11, "Name": "a"}
		{"Id": 22, "Name": "b"}
		{"Id": 33, "Name": "c"}
		{"Id": 44, "Name": "d"}
		{"Id": 55, "Name": "e"}
	`
	dec := json.NewDecoder(strings.NewReader(jsonStream))
	for {
		var m Message

		err := dec.Decode(&m)

		if err == io.EOF {
			break
		} else if err != nil {
			fmt.Println(err)
		} else {
			fmt.Printf("%d: %s\n", m.Id, m.Name)
		}
	}
}

// 结构体转json
func structToJson() {
	group := ColorGroup{
		Id:     1,
		Name:   "Reds",
		Colors: []string{"Crimson", "Red", "Ruby", "Maroon"},
	}
	b, err := json.Marshal(group)
	if err != nil {
		fmt.Println("error:", err)
	} else {
		os.Stdout.Write(b)
	}
}
```