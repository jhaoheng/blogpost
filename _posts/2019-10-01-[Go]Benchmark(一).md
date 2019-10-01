---
layout: post
title: 【Go】Benchmark(一)
date: 2019-10-01 16:31
categories: Golang
tags: Golang
---

# Synopsis
- 說明 benchmark 測試方式
- 測試 reflect.TypeOf 與 fmt.Sprintf("%T") 誰的效能比較好些

# go test -bench

```
func BenchmarkSliceNoPointers(b *testing.B) {
	b.ReportAllocs()
	for i := 0; i < b.N; i++ {
		slice := make([]MyStruct, 0, 100)
		for j := 0; j < 100; j++ {
			slice = append(slice, MyStruct{A: j, B: j + 1})
		}
	}
}
```

- `go test -bench . -count 10 > run.txt`
	- `-count` : 執行的次數, 每個 bench func()
	- `> run.txt` : 存入檔案

得到
```
BenchmarkSlicePointers-4   	  573025  2026 ns/op  1600 B/op  100 allocs/op
```

<!--more-->

- `-4` : 表示 CPU 核心數
- `573025` : 每一秒可跑幾次
- `2026` : 每一次處理耗費的時間
- `1600` : 每一次處理使用的 Bytes
- `100` : 每一次處理需要分配的記憶體 allocations

## example : main_test.go

> - 因提到 reflect 的效能很差，故在這邊做個測試
> - 測試 reflect.TypeOf() & fmt.Sprintf("%T") 哪個耗費的效能比較高
> - 順便附上他人的測試 : https://gist.github.com/crast/61779d00db7bfaa894c70d7693cee505

```
package main

import (
	"fmt"
	"reflect"
	"testing"
)

type MyStack struct {
	T string
}

func BenchmarkReflect(b *testing.B) {
	b.ReportAllocs()
	for index := 0; index < b.N; index++ {
		slice := make([]MyStack, 0, 100)
		for j := 0; j < 100; j++ {
			obj := reflect.TypeOf(j).String()
			slice = append(slice, MyStack{T: obj})
		}
	}
}

func BenchmarkSprintf(b *testing.B) {
	b.ReportAllocs()
	for index := 0; index < b.N; index++ {
		slice := make([]MyStack, 0, 100)
		for j := 0; j < 100; j++ {
			obj := fmt.Sprintf("%T", j)
			slice = append(slice, MyStack{T: obj})
		}
	}
}
```

- 執行 `go test -bench .`

```
goos: darwin
goarch: amd64
pkg: cycle
BenchmarkReflect-4        465621              2323 ns/op             792 B/op         99 allocs/op
BenchmarkSprintf-4        119300              9962 ns/op            1584 B/op        199 allocs/op
PASS
ok      cycle   2.405s
```

## 從執行結果分析
1. Reflect.TypeOf() 的效能比 fmt.Sprintf("%T") 來得好