---
layout: post
title: 【Go】type=nil, and value=nil
date: 2019-10-01 08:30
categories: Golang
tags: Golang
---

# Synopsis

> https://golang.org/doc/faq?fbclid=IwAR3iipOV2A1q6dOmqke545tNWouuReN1VY0A-3RGr2cdxwS_2SLNNKlO0ls#nil_error

- 有人問到 `Why is my nil error value not equal to nil?`
- 原因在於, 在指定類別時, nil 在 type = nil 跟 value = nil 代表不同的意義
	- `T=nil, V is not set`
	- `T=*int, V=nil`
- 而必須在 type = nil 的情況下，才使用 `if type == nil {}`

## Example

<!--more-->

```
package main

import (
	"errors"
	"fmt"
	"reflect"
)

/*
type error interface {
    Error() string
}
*/

func main() {
	/*
		type  : nil
		value :
	*/
	var a error = nil
	show(a)

	/*
		type  : *error
		value : nil
	*/
	var aa *error = nil
	show(aa)

	/*
		type  : *errors.errorString
		value : y
	*/
	b := errors.New("y")
	show(b)

	/*
		type  : nil
		value :
	*/
	c := errors.New("z")
	c = nil
	show(c)

	/*
		type  : int
		value : 0
	*/
	var d int
	show(d)

	/*
		type  : *int
		value : nil
	*/
	var dd *int
	show(dd)

	/*
		type  : string
		value : ""
	*/
	var e string
	show(e)

	/*
		type  : *string
		value : nil
	*/
	var ee *string
	show(ee)
}

func show(object interface{}) {
	fmt.Printf("%#v\n", object)
	fmt.Println("value", reflect.ValueOf(object))
	fmt.Println("type:", reflect.TypeOf(object))
	if reflect.TypeOf(object) == nil {
		fmt.Println("Its type is nil")
	} else {
		// if nil, nil pointer dereference
		fmt.Println("The Kind is ", reflect.TypeOf(object).Kind())
	}

	fmt.Println()
}
```