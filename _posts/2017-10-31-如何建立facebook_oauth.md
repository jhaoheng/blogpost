---
layout: post
title: "【facebook】建立 facebook oauth app"
date: 2017-10-31 08:44
categories: facebook app mobile web oauth
---

# 目標

mobile/web 端，透過 fb 進階 oauth 的方式，mobile/web 的 登入/註冊
當然 web 可以透過簡易的方式，透過 javascript 取得授權的 token，但此方法不在這邊討論
<!--more-->
## 建立 fb app

> 前期準備
> - 你的 server，可架設簡易的 server，並做出一個 api 接口，在 browser 上印出以下參數(測試方法 : 在 browser 上填入 api 即可)
> 	- Headers
> 	- Method
> 	- Rawbody
> 	- Scheme
> 	- URI
> 	- Fragment : `"<script>document.write(window.location.hash);</script>";`，只有透過 js 才能取得此參數
> 		- URL 組成 : scheme://host:port/path?query#fragment

1. 首先登入 `https://developers.facebook.com/`
2. 建立一個新的 fb app
3. 在左側 sidebar 中，選擇 新增產品 -> Facebook 登入 -> 設定，這樣就完成了新增
4. 在左側中，商品下會看到 『Facebook 登入』選擇設定
5. 在步驟四後的右側，**關閉**『對重新導向 URI 使用 Strict 模式』（原因在於此方法並沒有使用任何 SDK）
6. 在步驟四後的右側，『有效的 OAuth 重新導向 URI』中設定 **網站的 Host**，ex:`http://localhost`，注意，若你要轉址到 localhost，也是要設定 http://localhost
	- 為何要設定此方法，主要在於使用的方式會需要透過 facebook redirect 到你的伺服器，進行 註冊/登入，等行為，所以必須要驗證。

## OAuth coding 部分
> 前期準備
> - 在剛剛建立好的主控版取得以下參數
> 	- 應用程式編號
> 	- 應用程式密鑰
> 	- API 版本

- 正式開始取得授權 token : `https://jhaoheng.github.io/blogpost/facebook/app/mobile/web/facebook-%E9%80%B2%E9%9A%8E-%E7%99%BB%E5%85%A5/`
	1. mobile / web 端，呼叫 fb oauth(其中包含 redirect_uri)
	2. 透過 redirect_uri 轉到『你的 server』，這邊因 (1) 中的 `response_type` 設定方式，而有所不同
	3. 驗證 oauth_token
	4. 判斷『登入』、『註冊』，若無需用到 oauth_token 則，無須理會

## 透過 OAuth 使用 facebook graph-api

> 更多請參考 facebook graph api
> 可使用 `https://developers.facebook.com/tools/explorer/`

- me
	- url : `https://graph.facebook.com/{fb_api_version}/me?access_token={oauth_token}&fields={fields}`
	- {fb_api_version} : 請注意你的 fb_app 版本
	- {oauth_token} : 
	- {fields} : `email,name,id,picture.width(320).height(320)`

## 授權的用戶如何手動刪除 fb_app

1. 登入你的 facebook -> 設定 
2. 左側選擇『應用程式』-> 搜尋
3. 彈出的視窗可看到 『移除應用程式』