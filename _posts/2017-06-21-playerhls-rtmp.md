---
layout: post
title: '【Player】HLS.js / video.js / jwplayer'
date: 2017-06-21 06:59
comments: true
categories: 
---
# rtmp live

## 使用 video.js 的 issue

播放後，影片會在左上角，與 player size 不符合
- bug fixed : https://github.com/videojs/video-js-swf/issues/55
    1. 下載 https://github.com/digitalStyx/video-js-swf/blob/master/bin-debug/VideoJS.swf 到本地後
    2. 在 head 中加入
        ```
        <script>
          videojs.options.flash.swf = "VideoJS.swf";
        </script>
        ```
    3. 使用參考 : https://coolestguidesontheplanet.com/videodrome/videojs/

# hls playback

## 如何進行 offset

- document : https://tools.ietf.org/html/draft-pantos-http-live-streaming-18#section-4.3.5.2

1. version 6 以上
2. use like below : **#EXT-X-START:TIME-OFFSET=2**

```
#EXTM3U
#EXT-X-VERSION:7
#EXT-X-ALLOW-CACHE:YES
#EXT-X-TARGETDURATION:7
#EXT-X-MEDIA-SEQUENCE:1
#EXT-X-START:TIME-OFFSET=2

#EXTINF:7, 
1498008625670.ts
#EXTINF:6, 
1498008631711.ts
#EXTINF:6, 
1498008637927.ts

```
3. 注意播放器要支援解析, EX: `https://github.com/jhaoheng/html5_hls_test/blob/master/hls.html`

# jwplayer 使用 flash 播放 hls / rtmp

- jwplayer 提供 http / https 的網頁測試
  - http  : `http://demo.jwplayer.com/developer-tools/http-stream-tester/`
  - https : `https://developer.jwplayer.com/tools/stream-tester/`
- 要讓 jwplayer 透過 flash 支援 hls 的播放，需要加入 crossdomain.xml 檔案
	- 要注意的是，crossdomain.xml 的檔案內容，請勿一次全部 copy，盡量用輸入的，輸入完畢後，透過 browser 顯示，不然有可能會發生，檢查 cros 正確，但 jwplayer 會無法解析的問題，猜測應該是 copy 時，貼上帶入了奇怪的字元
- 若用 jwplayer，不指定類型，則 jwplayer 會自動根據內容自動切換 flash / html5 的播放類型，如此一來就不用特別使用 crossdomain.xml，只需在 nginx.conf 中設定 crossdomain 即可
