---
layout: post
title: '【SSH】設定簡易遠端登入'
date: 2016-10-14 08:01
comments: true
categories: SSH
---
# readme

- env : osx 10.11
- 使用 ssh 配置，便易登入遠端 server

# .ssh/config

> 所有的參數列表 https://linux.die.net/man/5/ssh_config，see PATTERNS

- Host  {本機辨識的名稱}
- HostName
- Port
- User

## 相關參數

- Host
- HostNmae
- User
- Port : 連線端口
- DynamicForward : 本地端口 

## 範例

> 登入遠端主機

1. 設定檔
```
# server
Host dev
HostName dev.max.com
Port 8888
User root
```
2. `ssh dev`

## 金鑰

> 若需設定伺服器金鑰，進行遠端登入

1. 在本機端生成金鑰 `$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
2. 上傳 pub\_key 到伺服器端
3. 將 key 匯入 `~/.ssh/authorized_keys`
4. `chmod 600 ~/.ssh/authorized_keys`
5. 設定伺服器端配置，`/etc/ssh/sshd_config`
	- PubkeyAuthentication yes
  - PasswordAuthentication no
6. 重啟 sshd 服務