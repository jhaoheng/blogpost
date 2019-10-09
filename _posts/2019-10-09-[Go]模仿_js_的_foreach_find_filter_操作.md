---
layout: post
title: 【Go】模仿 js 的 find, forEach & filter
date: 2019-10-09 16:56
categories: Golang
tags: Golang
---

# synopsis

- 基本上這是一間面試的時候，對方給的考題，只是讓我蠻不爽的是，只有十分鐘
- 而且我很少寫 js
- 因為一般來說，除非你習慣在『某個 code 下』執行某些事情，不然你不會特別去想這樣做
- 整個題目應該是在研究以下做法
	- type at struct & interface
	- func as a parameter
- 題目是『請模仿 js 的 find/forEach/filter 做出一樣的功能, 塞入任意的 array or slice, 可以透過 func 進行操作』
	- 基本上再增加一個 `type myObj interface{ filter() ... }` 就可以同時吃 array & slice
	- 以下是 slice 的做法

<!--more-->

# code

```
package main

import (
	"fmt"
)

type myslice []interface{}

type data struct {
	name string
	like string
	age  int
}

var datas = myslice{
	data{
		name: "Casper",
		like: "鍋燒意麵",
		age:  18,
	},
	data{
		name: "Wang",
		like: "炒麵",
		age:  24,
	},
	data{
		name: "Bobo",
		like: "蘿蔔泥",
		age:  1,
	},
	data{
		name: "滷蛋",
		like: "蘿蔔泥",
		age:  3,
	},
}

func main() {
	filterAgeThan5 := datas.filter(func(item interface{}, index int, newSlice *[]interface{}) {
		if item.(data).age > 5 {
			*newSlice = append(*newSlice, item)
		}
	})
	fmt.Println(filterAgeThan5)

	datas.forEach()

	findAgeThan5 := datas.find(func(item interface{}, index int, newSlice *[]interface{}) {
		if item.(data).age > 5 {
			*newSlice = append(*newSlice, item)
		}
	})
	fmt.Println(findAgeThan5)
}

type objFunc func(item interface{}, index int, newSlice *[]interface{})

func (datas *myslice) filter(o objFunc) []interface{} {
	var new []interface{}
	for i, obj := range *datas {
		o(obj, i, &new)
	}
	return new
}

// find() 與 filter() 很像，但 find() 只會回傳一次值，且是第一次為 true 的值。
func (datas *myslice) find(o objFunc) []interface{} {
	var new []interface{}
	for i, obj := range *datas {
		o(obj, i, &new)
		if len(new) > 0 {
			break
		}
	}
	return new
}

// 不會額外回傳值，只單純執行每個陣列內的物件或值。
func (datas *myslice) forEach() {
	fmt.Println(*datas)
}
```