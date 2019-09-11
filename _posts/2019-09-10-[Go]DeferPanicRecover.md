---
layout: post
title: 【Go】Defer, Panic & Recover
date: 2019-09-11 13:18
categories: Golang
tags: Golang
---

# Synopsis

- defer {動作} : 當前 func 結束後，會執行的動作。
- panic() : 當執行，會強迫中斷程式，並輸出錯誤訊息。
- recover() : 當 panic 發生後，無論錯誤在多深層的結構中，都會透過 recover() 取回當前的 error (不用逐層 return)，並讓程序繼續執行。

# 測試

- 在網路上多數的範例都是在於開啟檔案、連線失敗的使用方式
- 這邊提供另一種使用範例

<!--more-->

## 範例
> 假設我們在一個深層的遞迴中，發生了 panic，要回傳錯誤訊息，並讓程式不因為 panic，而中斷 process

1. 設定 Layer/TriggerError 變數，用 pointer 送入到 func
	- Layer : 用於觀察數字變動
	- TriggerError : 設定於該次的 function 呼叫時，產生 error，並啟動 panic
2. 透過 defer 在 work() 結束後, 執行動作
3. 透過 recover(), 在 panic() 發生後, 將 error 抓回來
4. 最後在 main() 中，輸出 "hello world"


<div>
    <iframe src="https://play.golang.org/p/ADukYaPNS_B" height="600" width="560" allowfullscreen="" frameborder="1">
    </iframe>
</div>

## 觀察結果

- 當刪除 `recover()` 這一段; 且 `TriggerError != 0`
	- 會造成 process 強迫中斷, 會看到錯誤訊息, 但無法看到接下來的訊息
	- work() 結束後, 也無法返回 main 中 
- 當 `TriggerError == 0`
	- 不會執行 recursion
	- 會看到沒有錯誤訊息
	- `The [Layer] value in 'defer' is : 999`
- `TriggerError != 0`
	- 輸出 panic 的 error 
	- 輸出 `The [Layer] value in 'defer' is : X`, X 是該 func 被執行的次數
	- 跳回 main(), 顯示 `Hello World`

## 結論

- 所以當要執行某個工作時，不用刻意的使用 return 回傳錯誤訊息
- 透過 defer, panic, recovery 可以設計出更優雅、容錯、可讀性高的程式

