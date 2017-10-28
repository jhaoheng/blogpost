---
layout: post
title: '透過 slack 訂閱 github release RSS'
date: 2017-08-03 14:46
comments: true
categories: 
---
## 目的

當使用越來越多的工具時，有時新版的 release 來不及跟上，變成版本落差極大，透過 slack rss bot 的幫忙，當 release 有更新時，透過 msg 通知

## 需要工具

- slack rss bot install : https://slack.com/apps/A0F81R7U7-rss
- github release rss
	1. 找到一個你想要訂閱的 github
	2. 點選 release 的 item
	3. 在 url 的尾端加上 `.atom` 即可。ex: https://github.com/xxxx/yyyy/releases.atom
	4. copy url to slack rss
	

