---
layout: post
title: '【xcode】framework 動/靜 diff'
date: 2016-06-26 06:21
comments: true
categories: 
---
# framework 靜態/動態 庫

## env

- xcode : 7.3.1
- ios 9

## diff

- 取出 bundle file
	- 靜態庫，無法取得 framework.plist 的資訊
	- 動態庫，可
- 包裝後的編譯檔，file 也會不同