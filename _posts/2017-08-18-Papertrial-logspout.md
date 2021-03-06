---
layout: post
title: "【log tool】papertrial+logspout"
date: 2017-08-18 07:51
comments: true
categories: Papertrial Logspout
---
## papertrail

- 申請一個 papertrail 帳號
- 建立一個 Log Destinations : 會得到 `logsx.papertrailapp.com:46888`

## logspout

- 建立 docker container
```
docker run --name="logspout" \
-d \
-e SYSLOG_HOSTNAME=test_name \
--volume=/var/run/docker.sock:/var/run/docker.sock \
gliderlabs/logspout \
{your_papertrail_log_destination}
```
- 測試
  - 以下兩種方法可以在 papertrial 的 log 中看到
    1. docker run -p 80:80 centos /bin/echo hello => 在 papertrial 印出 hello
    2. docker run -p 81:81 centos /bin/date => 在 papertrial 印出 日期
    3. docker run -i -d -p 80:80 centos /bin/bash
      - `docker exec -it [container_id] /bin/bash`
        - echo hello >> /proc/1/fd/1 => 在 papertrial 印出 hello
  - 注意 : docker run 用 -t 啟用的話，就不會被 logspout 給監聽到
  - 注意 : logging driver 只支援 `journald`, `json-file`