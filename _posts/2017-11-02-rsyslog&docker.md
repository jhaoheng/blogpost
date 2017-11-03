--- 
layout: post
title: "【log centralized】use rsyslog in docker"
date: 2017-11-02 08:12
categories: ubuntu rsyslog log docker
---

# 目標
code print log to syslog, and then put sent log to outside(log centralized)

<!--more-->
# 環境
- ubuntu 16.04
- docker
- php&nginx

# 安裝 rsyslog
1. 在 docker:container 中，安裝 `apt-get install rsyslog -y`
2. 設定檔
	- rsyslog.conf : 設定檔
	- rsyslog.d/ : 啟動預載檔
3. log 位置 : `/var/log`，可參考啟動預載檔

# docker 

- 做法有兩種
	1. 一種是使用 docker:remote_syslog 將 syslog 的檔案丟到外面系統中央化
		- 詳情參考 `https://jhaoheng.github.io/blogpost/2458676/`
	2. 另一種是用 rsyslog 本身支援直接指定目標位置
- 另外 docker log 使用預設的即可支援 syslog

# 測試
## bash
- 方法一 : 直接寫到 syslog
	1. 進入 container
	2. 找到 syslog 的位置
	3. 直接寫入 `echo "hello" >> syslog`
- 方法二 : 透過 logger
	1. 進入 container
	2. `logger -s "world"`
- 檢查 : 
	1. 找到 syslog 的位置
	2. `tail -f syslog`

## php code
```
openlog("response", LOG_PID, LOG_SYSLOG);
syslog(LOG_INFO, json_encode($log));
closelog();
```

此方法會自動將 log 寫到 syslog（透過 rsyslog）中，再透過其他工具（remote_log/rsyslog）放到 log centralized(papertrail) pool