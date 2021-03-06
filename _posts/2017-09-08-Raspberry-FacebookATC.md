---
layout: post
title: '【Raspberry pi 3】安裝 facebook_atc'
date: 2017-09-08 14:08
comments: true
categories: Raspberry FacebookATC
---
## install & setting

1. 建立好 wifi ap
2. 安裝 ATC : `http://facebook.github.io/augmented-traffic-control/docs/install.html`
3. 安裝完畢 atc django-ui 後，在執行 atcd 前，必須先知道 wan & lan 的端口
	- 透過 `route` 查詢
	- 啟動 `sudo atcd --atcd-lan xxxx --atcd-wan xxxx`
4. 執行 python manage.py runserver 0.0.0.0:8000
5. 開啟瀏覽器，會看到成功畫面
	- 若你的 atcd server 執行失敗，會產生授權錯誤的推播通知

## use

1. 開啟瀏覽器並打開 atc_demo_ui 的管理介面
2. 主要設置的參數有：
	- 網絡帶寬（bandwidth）
	- 延遲（latency）
	- 丟包率（packet loss）
	- 錯包率（corrupted packets）
	- 亂序率（packets ordering）
3. 新增 new profile 與 shaping settings 後，可在 Profiles 中看到設定的參數值，選擇後再開啟最上方的 trun on 即可
4. Network Condition Profiles 參考
	- https://github.com/tylertreat/Comcast#network-condition-profiles
  - https://github.com/facebook/augmented-traffic-control/tree/master/utils/profiles
5. 測試 : 連上 wifi，設定 bandwidth=1，turn on，此時若網路沒有延遲，則可能是你的 atcd wan&lan 設定相反

# 測試心得
1. 因為 atc 可以個別針對各類型的裝置進行限速的行為，所以今天要限制某台 iphone，請透過該台 iphone 連到 atcd-ui 中，選擇 profile (需要的網路情境)，再開啟，即可，根據測試，不同的裝置，可以設定不同的情境，不會互相影響
2. 如何遠端控制它台裝置？
	1. 需要先授權，授權完畢，重新整理頁面，就會看到授權的電腦
  2. 參考 : https://github.com/facebook/augmented-traffic-control/tree/master/atc/django-atc-api
  3. 關於設定的 json 設定，我是先在 ui 的頁面中，copy 設定檔，再透過 api 的方式進行設定