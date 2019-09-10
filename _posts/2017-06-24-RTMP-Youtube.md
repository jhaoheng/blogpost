---
layout: post
title: '【RTMP】youtube to rtmp'
date: 2017-06-24 12:55
comments: true
categories: RTMP
---
# 目的
將 youtube 的影片傳給 rtmp server 進行串流

# how
1. 找到 youtube 的影片
2. 進行轉檔為 mp4 : https://www.onlinevideoconverter.com/zh/success?id=b1d3e4d3e4d3g6b1
    - 或者利用各種轉檔方式
    - 可下載為 file 也可直接抓取 url (但注意 url 會過期)
    - 或者把影片存成 mp4 後，放到 server 上
3. `docker run --rm jrottenberg/ffmpeg:ubuntu -re -i http://... -vcodec libx264 -preset ultrafast -acodec libfaac -f flv rtmp:...`
    - `-re` : Read input at native frame rate. Mainly used to simulate a grab device.  or live input stream (e.g. when reading from a file). Should not be used with actual grab devices or live input streams (where it can cause packet loss).  By default ffmpeg attempts to read the input(s) as fast as possible.  This option will slow down the reading of the input(s) to the native frame rate of the input(s). It is useful for real-time output (e.g. live streaming).
    - `http://...` : 輸入的 mp4 影片位置，也可用 file 來替代
    - `rmpt://...` : 要串流的 rtmp server 位置