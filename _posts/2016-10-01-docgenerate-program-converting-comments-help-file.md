---
layout: post
title: '【docGenerate】程式註解轉換成說明檔'
date: 2016-10-01 03:10
comments: true
categories: 
---
## source

https://github.com/jhaoheng/docGenerate/tree/master

## 使用環境

- v0.1-release : 單純支援bash script 的環境
- v0.2-release : 支援 config.json (需安裝 jq)

## 目的

不論是寫何種語言，都希望能把當下程式中的註解，直接轉譯為 .md，並且能最小化註解風格格式化
方便性為，若為 restful-api 對外的文件，則可直接將要公開的文件內容輸出為 .md 檔案
降低文件的重複編輯性，加速工程人員對於文件註解的習慣與完整性

Hope this script could help accelerate development and persist in good comment habit.
Do not need to maintain twice version between code and document.
When developer write down comment in code context, it mean that you finish the document at the same time. 