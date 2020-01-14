---
layout: post
title: 【Go】TestMain
date: 2020-01-14 14:47
categories: Golang
tags: Golang
---

# Synopsis
- 在 testing 中（https://golang.org/pkg/testing/#hdr-Main）有一段描述 TestMain
- 直覺反應會覺得跟 go main 有關係, 但實際上完全沒有關係
- 以下探討幾個使用 TestMain 的技巧

```
func TestMain(m *testing.M) {
	// call flag.Parse() here if TestMain uses flags
	os.Exit(m.Run())
}
```

<!--more-->

# TestMain(m *testing.M)

1. 必須要用 TestMain 當作 func name, 否則會出錯
2. 其中的 m.Run(), 為執行 `此份 test` 時, 會執行的 main thread, 邏輯是 : 執行 m.Run() 前 -> m.Run() 負責驅動 Testing code -> 執行 m.Run() 後

```
package pkgs

import (
	"fmt"
	"testing"
)

func TestMain(m *testing.M) {
	fmt.Println("before")
	m.Run()
	fmt.Println("after")
}

func Test_Code1(t *testing.T) {
	fmt.Println("測試 code 1")
}

func Test_Code2(t *testing.T) {
	fmt.Println("測試 code 2")
}
``` 

3. 當執行 `go test -run Test_Code1 ` 會得到 

```
before
測試 code 1
PASS
after
```

4. 當執行 `go test`

```
before
測試 code 1
測試 code 2
PASS
after
```

# 何時會適合用到 TestMain?
- 當需要載入一個 Test 都需用到的服務
- 當你的測試需要用到 flag 時
- 當你的測試需要在結束時，執行某些事情時
- ...