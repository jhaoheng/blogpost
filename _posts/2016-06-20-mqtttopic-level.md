---
layout: post
title: '【MQTT】Topic Level'
date: 2016-06-20 13:16
comments: true
categories: 
---
# MQTT Wildcards(萬用字元)

透過 Wildcards 可以一次訂閱想要的主題，Wildcards 只能運用於 sub 不能用於 pub

`myhome/ground/room/temperature`

每一個 `/` 的兩邊，代表的兩個 topic level

## Single Lever : + (單一層級萬用字元)

Single Level 是指取代此一層的主題(topic level)，`+` 代表此 topic level 有著 single level wildcard 的特性

`myhome/groundfloor/+/temperature`

在此標誌下，以下均為相同的主題

```
myhome/groundfloor/a/temperature
myhome/groundfloor/b/temperature
myhome/groundfloor/c/temperature
```

以下為不相同的訂閱主題

```
myhome/ground/a/temperature
myhome/groundfloor/a/brightness
```

ex2: 

- `home/[position]/[devices]/temperature` : 訂閱'家中單一房間，單一個 device 的溫度'
- `home/+/+/temperature` : 訂閱'家中所有房間中的裝置的溫度'


## Multi Level : #

Multi Level 總是放在最後 : `myhome/groundfloor/#`

以下為相同的主題
```
myhome/groundfloor/livingroom/temperature
myhome/groundfloor/livingroom/brightness
```

以下為不相同的主題
```
myhome/outside/temperature
```

## Topics beginning with $

這是系統預設的識別符，專門 publish 給系統，client 無法使用

# 注意
- `/` 符號，別放在第一位，ex : `/ex1/ex2`
- 在 topic level 中，不要空白
- 保持你的 topic level 簡短與簡單明瞭
- 使用 ASC II 字符，並免使用無法印出的符號
- 透過在 topic level 中，使用 clientId，可有效區分『消息來源』與『訊息區別』
	- client1 只能被授權 publish 透過 `client1/status`，不被允許 publish 給 `client2/status`
- 不使用 `#`
- 注意擴展性、延展性
- 使用特定的 topics，取代攏統的設定
	- 使用 `myhome/livingroom/temperature`,`myhome/livingroom/brightness` 與 `myhome/livingroom/humidity`，避免使用 `myhome/livingroom/` 來接收所有的訊息