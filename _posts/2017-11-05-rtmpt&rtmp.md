---
layout: post
title: "【nginx】rtmpt(tunnel by http:80) to rtmp:1935"
date: 2017-11-05 09:05
categories: docker nginx rtmpt rtmp ossrs 
---

# 目標
1. 透過 nginx，將 流 透過 rtmpt:80 的 protocol 轉送給 rtmp:1935 protocol
2. 推流 ffmpeg / OBS, protocol : rtmpt
3. 收流 VLC player, protocol : rtmp
4. 額外測試 : 送 rtmpt 給 ossrs
5. 延伸
	1. 透過 rtmpts (tunnel by https)，加入 ssl
	2. 透過 rtmps，直接接收 rtmps 流，並 proxy 轉送給 ossrs
<!--more-->

# env

- push streaming tool
	- ffmpeg 3.4 : 
		- 將桌面影像上傳 : `ffmpeg -f avfoundation -i "1" -vcodec libx264 -preset ultrafast -acodec libfaac -f flv rtmpt://localhost/demo/desktop`
	- OBS
		- 將制式的 mp4 檔案丟到 `rtmpt://localhost/demo/video`
	- wrong : `[tcp @ 0x7f830d5153c0] Connection to tcp://localhost:80 failed (Connection refused), trying next address`
		- 會自動檢查 `127.0.0.1`
- watch streaming tool
	- VLC player
- docker :
	- ossrs 
	- nginx:latest (1.13.6)
		- rtmp server
		- rtmpt server
- compiler first
	- nginx-rtmp-module : `https://github.com/arut/nginx-rtmp-module`
	- nginx-rtmpt-proxy-module : `https://github.com/kwojtek/nginx-rtmpt-proxy-module`

# nginx. conf : rtmpt & rtmp
```
user  nginx;
worker_processes  1;

# should add this to enable rtmpt : 
# https://github.com/kwojtek/nginx-rtmpt-proxy-module
load_module modules/ngx_rtmpt_proxy_module.so;

load_module modules/ngx_rtmp_module.so;

error_log  /var/log/nginx/error.log warn;

pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    keepalive_requests  4294967295;
    access_log  /var/log/nginx/access.log;
    server {
        listen       80;

        location ~ (^/open/1$|^/idle/.*/.*$|^/send/.*/.*$|^/close/.*/.*$) {
            rtmpt_proxy on;
            rtmpt_proxy_target localhost:1935; # 轉給 ossrs 請設定於此
            rtmpt_proxy_rtmp_timeout 2; 
            rtmpt_proxy_http_timeout 5;

            add_header Cache-Control no-cache;
            access_log on;

            proxy_buffers               32 4m;
            proxy_busy_buffers_size     25m;
            proxy_buffer_size           512k;
            proxy_ignore_headers        "Cache-Control" "Expires";
            proxy_max_temp_file_size    0;
            proxy_set_header            Host $host;
            proxy_set_header            X-Real-IP $remote_addr;
            proxy_set_header            X-Forwarded-For $proxy_add_x_forwarded_for;
            client_max_body_size        1024m;
            client_body_buffer_size     4m;
            proxy_connect_timeout       300;
            proxy_read_timeout          300;
            proxy_send_timeout          300;
            proxy_intercept_errors      off;
        }    
        location /fcs/ident2 {
            return 200;
        }
   }
}
 
rtmp {
    access_log  /var/log/nginx/access.log;
    server {
        listen 1935;
        chunk_size 4096;

        application demo {
                live on;
                record off;
        }
    }
}
```
