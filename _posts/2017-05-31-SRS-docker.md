---
layout: post
title: "【SRS-release2.0】docker with centos7.3"
date: 2017-05-31 03:50
comments: true
categories: SRS Docker
---
# 環境
- srs 2.0
- centos7.3

# 完成品
- https://hub.docker.com/r/jhaoheng/srs-release2.0/
- `docker run -d -t -p 1935:1935 {image}`
- 位置 : `/opt/srs/trunk`，執行 srs 即可

# base
- `docker pull centos:latest`
- `docker run -d -t -p 1935:1935 [centos:latest]`
- `docker exec -it [container] /bin/bash`

# 安裝
- `yum update`
- required
	- `yum install -y gcc`
	- `yum install -y gcc-c++`
	- `yum install -y make`
	- `yum install -y patch`
	- `yum install -y unzip`
	- `yum install -y pcre-devel`
	- `yum install -y automake`
	- `yum install -y libtool`
	- `yum install -y zlib-devel`
- git clone [source]
- 安裝 `./trunk/3rdparty/CherryPy-3.2.4`
	- `unzip ./trunk/3rdparty/CherryPy-3.2.4.zip && mv CherryPy-3.2.4 ../objs/`
- compile srs : `./configure --full --with-ffmpeg --with-research && make`
	- `./configure -h` : 可查看所有安裝選項
- start srs : `./objs/srs -c conf/srs.conf`

# 測試 
- `tail -f ./trunk/srs.log`
- 將桌面的畫面推送給 docker:srs
  - `ffmpeg -f avfoundation -i "1" -vcodec libx264 -preset ultrafast -acodec libfaac -f flv rtmp://localhost:1935/live/desktop`
- 透過 VLC 播放 `rtmp://localhost:1935/live/desktop`