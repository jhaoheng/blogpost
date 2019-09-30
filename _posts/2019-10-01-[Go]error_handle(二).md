---
layout: post
title: 【Go】panic(), defer, and recover()【二】
date: 2019-10-01 04:32
categories: Golang
tags: Golang
---

# Synopsis

> 隨便舉個例子，如何使用 error handle in golang

```
package main
 
import (
    "log"
    "os"
)
 
func main() {
    file, err := os.Open("file.txt")
    if err != nil {
        log.Fatal(err)
    }
 
    // Do something on file object.
}
```

- 在此程式碼中，會直接在 `log.Fatal` 直接中斷，這是一個無法繼續下去的範例
- Q : How to keep working? with error handle.

# panic(), defer, and recover()

- 在之前的文章中  [Defer,Panic,Recover]({{ site.url }}{% link _posts/2019-09-10-[Go]DeferPanicRecover.md %}) ，可以透過此機制，取得錯誤資訊，並透過 recover() 讓程式碼繼續執行
- 如何應用在此範例中？

<!--more-->

## 利用 panic, defer, recover 重構步驟
1. 將 `file, err := os.Open("file.txt")`, 建立另一個 func(), 放入其中
2. 建立 errorHandle(), 並在 (1) 中的 func(), 利用 defer 進行呼叫

```
package main

import (
	"fmt"
	"log"
	"os"
)

func main() {
	openFile("file.text")
	fmt.Println("keep working....")

}

func errorHandle() {
	if err := recover(); err != nil {
		log.Println(err) // 返回 panic(err) 的錯誤訊息
	}
}

func openFile(filename string) {
	defer errorHandle()
	file, err := os.Open(filename)
	defer file.Close()
	if err != nil {
		panic(err)
	}

	fmt.Println("success")
}
```

- Q : 如何在錯誤中，得知，這是哪一個 func 造成的錯誤?

## Q : 如何在錯誤中，得知，這是哪一個 func 造成的錯誤?

```
func errorHandle() {
	if err := recover(); err != nil {
		log.Println(err, ". Work at:", printFuncName()) // 返回 panic(err) 的錯誤訊息
	}
}

/*
Print func name
*/
func printFuncName() string {
	fpcs := make([]uintptr, 1)

	// Skip 4 levels to get the caller
	n := runtime.Callers(4, fpcs)
	if n == 0 {
		fmt.Println("MSG: NO CALLER")
	}

	caller := runtime.FuncForPC(fpcs[0] - 1)
	if caller == nil {
		fmt.Println("MSG CALLER WAS NIL")
	}

	// Print the name of the function
	return caller.Name()
}
```

- Q : 如何讓程式碼看起來像是 try{}catch{} 般使用?

## Q : 如何讓程式碼看起來像是 try{}catch{} 般使用?
> 利用 `type errorHandle func()`
> 以下是片段程式碼, 可以改成 `func try(action, catch)` 的方式, 可能更適合某些人

```
func main() {
	try("file.text", catch)
	fmt.Println("keep working....")

}

type errorHandle func()

func try(filename string, e errorHandle) {

	defer e()
	file, err := os.Open(filename)
	defer file.Close()
	if err != nil {
		panic(err)
	}

	fmt.Println("success")
}

/*
Show error
*/
func catch() {
	if err := recover(); err != nil {
		log.Println(err, ". Work at:", printFuncName()) // 返回 panic(err) 的錯誤訊息
	}
}
```

- Q : 上述範例是利用 open 後產生的錯誤訊息，但如何自訂義錯誤?

## Q : 上述範例是利用 open 後產生的錯誤訊息，但如何自訂義錯誤?

```
func otherFunc() {
	defer catch()

	// define customized error
	err := errors.New("Some error")
	if err != nil {
		panic(err)
	}
}
```


# final example code

```
package main

import (
	"errors"
	"fmt"
	"log"
	"os"
	"runtime"
)

func main() {
	try("file.txt", catch) 

	otherFunc()
	fmt.Println("keep working....")

}

type errorHandle func()

func try(filename string, e errorHandle) {

	defer e()
	file, err := os.Open(filename)
	defer file.Close()
	if err != nil {
		panic(err)
	}

	fmt.Println("success")
}

func otherFunc() {
	defer catch()

	// define customized error
	err := errors.New("Some error")
	if err != nil {
		panic(err)
	}
}

/*
Show error
*/
func catch() {
	if err := recover(); err != nil {
		log.Println(err, ". Work at:", printFuncName()) // 返回 panic(err) 的錯誤訊息
	}
}

/*
Print func name
*/
func printFuncName() string {
	fpcs := make([]uintptr, 1)

	// Skip 4 levels to get the caller
	n := runtime.Callers(4, fpcs)
	if n == 0 {
		fmt.Println("MSG: NO CALLER")
	}

	caller := runtime.FuncForPC(fpcs[0] - 1)
	if caller == nil {
		fmt.Println("MSG CALLER WAS NIL")
	}

	// Print the name of the function
	return caller.Name()
}
```