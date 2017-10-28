---
layout: post
title: '【iOS】APNS with HTTP/2'
date: 2016-07-05 06:29
comments: true
categories: 
---
## insatll (mac)

- `brew install curl --with-nghttp2` 
- `brew link curl --force`

## use
- `./curl --version`

```
curl 7.48.0 (x86_64-apple-darwin15.5.0) libcurl/7.48.0 OpenSSL/1.0.2g zlib/1.2.5 nghttp2/1.9.1
Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtsp smb smbs smtp smtps telnet tftp 
Features: IPv6 Largefile NTLM NTLM_WB SSL libz TLS-SRP HTTP2 UnixSockets 
```

- exec : `./curl -v -d '{"aps":{"alert":"Test Push","sound":"default"}}' --cert [xxx.pem] -H "apns-topic: [bundle_id]" --http2 https://api.development.push.apple.com/3/device/[token]`