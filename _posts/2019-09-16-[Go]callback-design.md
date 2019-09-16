---
layout: post
title: 【Go】callback, func as a type
date: 2019-09-16 14:52
categories: Golang
tags: Golang
---
# 宣告
1. func as a type : `type callbackHandler func(name string)`
2. 在宣告 func 時, 將 func 放到參數位置 : `func DoSomething(callback callbackHandler) {...}`

# 用途設計
- 在 objective-c 中的 complete, 通常做的都是異步的處理, ex: 等待 api 的回應過程中, 處理其他事情, 像 closure
- 所以用途設計應該可以用在
	1. 有 N 個 func, funcN 依賴 funcMain 的處理結果, 並且 funcN 可能還有其他事情需要處理
	2. callbackHandler 的設計, 可以將多個 func 合併在一起, 個別制定 callbackHandler 內容, 並且回傳固定的返回值. Ex: error

```
[原本]
sum := funcMain(args []int)
temp := funcN(sum)

[整合]
type N funcN(sum int)
funcMain(args []int, n N) {
	sum := 0
	for _, v := range args {
		sum = v + sum
	}
	n(sum)
}
---
var temp int
funcMain(args []int, funcN(sum int) {
	fmt.Println(sum)
	temp = sum
})
```
<!--more-->

# 範例設計
1. 進行 sigin, 查詢資料庫
2. 查詢完畢, 回傳

```pkg
package pkg

import (
	"errors"
	"fmt"
	"time"
)

type Person struct{}

type completeCallbackHandler func(err error)

func (p *Person) Signin(account, password string, c chan string, complete completeCallbackHandler) {
	fmt.Printf("Verify account = %s, password = %s, from database\n", account, password)

	var err error
	if false {
		err = errors.New("Something wrong")
	}
	complete(err)

	go func() {
		time.Sleep(time.Second * 2)
		c <- "cmd : send notification to firebase"
	}()
}
```

```main
package main

import (
	"callback/pkg"
	"fmt"
	"time"
)

type ResponseObjs struct {
	statusCode int
	err        string
}

var channelOfRobot chan string = make(chan string)

func main() {

	go worker()

	p := pkg.Person{}

	//
	r := ResponseObjs{}
	p.Signin("xxx@gmail.com", "1234", channelOfRobot, func(err error) {
		if err != nil {
			r.statusCode = 500
			r.err = err.Error()
		} else {
			r.statusCode = 200
			r.err = ""
		}
	})
	fmt.Printf("return : %+v\n", r)

	//
	fmt.Println("system handle other process....")

	// 按下任意鍵停止
	var pause string
	fmt.Scanln(&pause)
}

func worker() {
	for {
		select {
		case cmd := <-channelOfRobot:
			fmt.Printf("Exec => %+v\n", cmd)
		default:
			time.Sleep(time.Second)
		}
	}
}
```