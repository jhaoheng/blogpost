---
layout: post
title: '【PHP-MAMP】pcntl 安裝'
date: 2017-04-05 01:07
comments: true
categories: 
---
# 環境
- osx
- php7.0.8
- MAMP 3.5.2

# flow
0. `brew search pcntl`，找尋要安裝的版本
1. `brew install php70-pcntl`，得到 pcntl 的安裝位置 => pcntl__path
2. 安裝完畢後，確認 php, `which php`，得到路徑 => php__path
3. ls -al 路徑(php__path)，確認 php 的 source 指到的是 MAMP 的 php 路徑
4. 在 pcntl__path 中，取得 pcntl.so 的檔案，copy 到 MAMP 的 extension 中
5. 編輯 localhost 的 php.ini 與 MAMP 的 php.ini，加入 `extension=pcntl.so`
6. 在 terminal 中，`php -i | grep pcntl`，可看到版本訊息