---
layout: post
title: 【Raspberry】use rtmp to media-server
date: 2018-01-18 16:30
categories: raspberry rtmp
---

# SYNOPSIS
> 目的 : 透過 gstreamer 將，picamera 的 stream 透過 rtmp 傳給 media-server

1. 安裝主程式
2. 安裝 pulgin 套件
	- 下載 gst-rpicamsrc 編譯
	- 推送 rtmp 到 media-server
3. 觀看 live

<!--more-->

# gstreamer

- 主程式 : `apt-get install gstreamer1.0-tools`
- plugin repo : use https://github.com/thaytan/gst-rpicamsrc
	- 透過 `apt-get install gstreamer1.0-tools` 安裝的方式，無法抓取 picamera 的 stream
- install dep
	- `apt-get install autoconf automake libtool`
	- `apt-get install libgstreamer1.0-dev`
	- `apt-get install libgstreamer-plugins-base1.0-dev`
	- `apt-get install libraspberrypi-dev`
- build gst-rpicamsrc
	1. `git clone https://github.com/thaytan/gst-rpicamsrc.git`
	2. `cd gst-rpicamsrc/`
	3. `./autogen.sh --prefix=/usr --libdir=/usr/lib/arm-linux-gnueabihf/`
	4. `make`
	5. `reboot`
- start push stream

```
gst-launch-1.0 -v rpicamsrc \
 !'video/x-h264,width=1280,height=720,framerate=15/1' \
 ! h264parse ! flvmux \
 ! rtmpsink location="rtmp://yourstreamserver/live/pi"
```

# 建立 media-server
- 可使用 nginx rtmp-module 進行架設, [demo](https://github.com/jhaoheng/media-server-docker)

# 觀看 live
- 可用 vlc 觀看 rtmp 
- 或者用簡易版的 [docker-player](https://github.com/jhaoheng/html5_player_test)

# 參考文章
- [How to stream from RaspberryPi PiCamera](http://c.wensheng.org/2017/05/18/stream-from-raspberrypi/)