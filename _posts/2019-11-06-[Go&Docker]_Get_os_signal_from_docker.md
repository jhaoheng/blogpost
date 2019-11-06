---
layout: post
title: 【Go】Go bin get os signal from docker
date: 2019-11-06 11:37
categories: Golang
tags: Golang
---

# Synopsis

- 建立 docker image, 讓 bin 在 PID=1 的狀態下執行
- 接收 os signal 的訊號, 讓程式得知結束前, 需要做什麼事情
- code example : https://github.com/jhaoheng/docker_op_books/tree/master/example/run_cmd_at_pid_1

<!--more-->

## Dockerfile 需要注意的地方

- 若在 CMD 中, 直接用 `CMD: {your_bin}`, 會得到錯誤的啟動, for example:

![img-wrong](https://raw.githubusercontent.com/jhaoheng/docker_op_books/master/example/run_cmd_at_pid_1/assets/wrong.png)

- 在 dockerfile 中的 CMD 需要用 `exec {your_bin}` 來執行, 正確會得到

![img-right](https://github.com/jhaoheng/docker_op_books/blob/master/example/run_cmd_at_pid_1/assets/right.png?raw=true)