---
layout: post
title: '【LINE】develop intro'
date: 2016-04-15 02:06
comments: true
categories: [line]
---
https://developers.line.me/

# 申請與註冊開發

1. 至 Channels，登入
	- 若是第一次登入，會出現無 Channels 的頁面，請點選 『Business Center』
		- 一個商用帳號，只能選擇一種開發
	- 建立商用帳號後，可選三種開發
		- LINE@
			- LINE@ 提供針對公司或經營者使用的LINE帳號，不但能讓商業運作及訊息傳達更加活躍， 還具備了能夠將企業、品牌以及商品魅力傳達給顧客的各種功能。
			- 群發訊息
			- 1 對 1 聊天 : 能夠直接透過LINE聊天室溝通，回覆來自顧客
			- 發送動態消息 : 將資訊投稿至顧客或粉絲的動態消息上
			- 免費方案 : 每月1,000則以內
			- 推廣方案 : 無限制
		- LINE Login
			- LINE Login是可與您的事業內容結合，讓用戶能夠以LINE帳號登入並使用的功能。
			- 透過LINE SDK for iOS / Android，將能夠在Native Application使用LINE Login功能。
			- 透過LINE所提供的OAuth2認證，將能夠在Web Application使用LINE Login功能。
		- BOT API Trial Account
			- BOT API Trial Account是一項能夠讓用戶針對您所提供的事業內容進行雙向溝通的功能。
			- BOT API將透過LINE伺服器，在您的伺服器與LINE應用程式間互相收發資訊。將活用JSON形式的API進行通訊請求。
			- 可傳送訊息給將您帳號加為好友的用戶。將透過API收發所有的訊息。
			- 可傳送含有網站縮圖及連結的訊息，能夠有效地將用戶引導至網站中。
			- 使用條件
				- 測試時，上限傳送人數 50 人

# Develop
## LINE@

- 透過 business center 進行申請

### 認證
**若認證失敗，則會轉為一般帳號**

- [認證帳號與一般帳號](http://at-blog.line.me/tw/archives/36225665.html)
- 5~7 個審核工作天

## Line Login

- 透過 business center 進行申請
- App icon : 在申請帳號服務時的 name 與 image 即為此 login app 的預設資料
- 建立好後，可設定以下參數跟下載開發 sdk (目前 iOS 版本 3.1.17)
	- NATIVE_APP
		- iOS Bundle ID
		- iOS Scheme
		- Android Package Name : 
		- Android Package Signature
		- Android Scheme
	- WEB
		- Authentication domain

**Note: The download link is displayed only after the Channel status becomes DEVELOPING.**

## Line bot

- 透過 business center 進行申請
- basic
	- Name
	- App icon
	- Callback URL
- Server IP WhiteList : 可針對單一的 ip 進行設定，也可以透過子網路遮罩，設定一整組的範圍
		
