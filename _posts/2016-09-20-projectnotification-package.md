---
layout: post
title: '【Project】iOS Notification Package'
date: 2016-09-20 14:59
comments: true
categories: 
---
## 目的

每次處理 apns，總是會發生很多意外，token 錯誤，憑證搞錯....
此兩個 app，主要就是在解決測試環境的問題

## application

- [APNS\_cer\_build](https://github.com/jhaoheng/APNS_cer_build)
  - 收納憑證
	- 建立憑證
	- 測試憑證
	- 測試 apns server
	- 測試推播
- [apnsReceiver](https://github.com/jhaoheng/apnsReceiver)
	- 查看目前 ipa 的版本為
		- developement
		- Ad-Hoc
		- App Store
		- Enterprise
	- 查看 token
	- 寄送 token
	- 接收 push