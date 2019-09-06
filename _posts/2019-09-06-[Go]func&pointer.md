---
layout: post
title: 【Golang】func & pointer
date: 2019-09-06 14:31
categories: Golang
---

# kind
> Different kinds use a pointer to change the value in Func.

- map
- slice
- array
- string
- struct

<div>
    <iframe src="https://play.golang.org/p/sUiGkLDt8vJ" height="315" width="560" allowfullscreen="" frameborder="1">
    </iframe>
</div>

<!--more-->

```
package main

import "fmt"

func main() {
	// map
	var mapObj = map[int]string{}
	mapObj[0] = "max"
	updateMapObj(mapObj)
	fmt.Println("   map =>", mapObj)

	// slice
	sliceObj := []string{
		0: "max",
	}
	updateSliceObj(sliceObj)
	fmt.Println(" slice =>", sliceObj) // [sunny]

	// array
	arrayObject := [1]string{"max"}
	updateArrayObj(&arrayObject)
	fmt.Println(" array =>", arrayObject) // [sunny]

	// string
	var stringObj string
	stringObj = "max"
	updateStringObj(&stringObj)
	fmt.Println("string =>", stringObj) // 999

	// struct
	var srtuctObj StructObject
	srtuctObj.name = "max"
	updateStructObj(&srtuctObj)
	fmt.Println("struct =>", srtuctObj) // {sunny}
}

func updateMapObj(mapObj map[int]string) {
	mapObj[0] = "sunny"
}

func updateArrayObj(arrayObject *[1]string) {
	arrayObject[0] = "sunny"
}

func updateSliceObj(sliceObj []string) {
	sliceObj[0] = "sunny"
}

func updateStringObj(stringObj *string) {
	*stringObj = "sunny"
}

type StructObject struct {
	name string
}

func updateStructObj(srtuctObj *StructObject) {
	srtuctObj.name = "sunny"
}
```