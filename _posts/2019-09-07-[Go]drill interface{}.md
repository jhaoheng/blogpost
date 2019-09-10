---
layout: post
title: 【Go】Drill interface{}
date: 2019-09-07 16:17
categories: Golang
tags: Golang
---

# interface{}

## Explore
> When set object is an interface{}

1. Set object is an interface{}
2. Use pointer to set object value is int / string / struct / slice
3. Print kind / address / type / value

## conclusion
- The `interface{}` is changed by each time setting, but the address is not changed.

## for example

<div>
    <iframe src="https://play.golang.org/p/7p1DnXwQ9EL" height="315" width="600" allowfullscreen="" frameborder="1">
    </iframe>
</div>

<!--more-->

#### source
```
package main

import (
	"fmt"
	"reflect"
)

type S struct {
	x int
	y int
}

func main() {

	var i interface{}
	printInfo(&i)

	setInt(&i)
	setStr(&i)
	setStruct(&i)
	setSlice(&i)
}

func setInt(i *interface{}) {
	*i = 999
	printInfo(i)
}

func setStr(i *interface{}) {
	*i = "str"
	printInfo(i)
}

func setStruct(i *interface{}) {
	*i = S{
		x: 1,
		y: 2,
	}
	printInfo(i)
}

func setSlice(i *interface{}) {
	*i = []int{1, 2, 3, 4}
	printInfo(i)
}

func printInfo(i *interface{}) {

	objKind := reflect.ValueOf(*i).Kind()
	objType := fmt.Sprintf("%T", *i)
	objAddress := fmt.Sprintf("%p", *&i)
	objValue := *i
	fmt.Printf("[Kind is : %v]\n", objKind)
	fmt.Printf("% 15v %v\n", "Address =>", objAddress)
	fmt.Printf("% 15s %v\n", "Type =>", objType)
	fmt.Printf("% 15s %+v\n", "Value =>", objValue)
}
```