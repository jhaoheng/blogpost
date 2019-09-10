---
layout: post
title: "【MongoDB-php7-drive】centos7"
date: 2017-05-11 14:40
comments: true
categories: 
---
## 環境
php7
centos7
MongoDB.so , 非 mongo.so

## 安裝 mongodb
1. yum install openssl-devel -y
2. wget http://pear.php.net/go-pear.phar
3. php go-pear.phar
4. vi `which pecl` => 拿掉 -n , 原因 https://serverfault.com/questions/589877/pecl-command-produces-long-list-of-errors
5. pecl install mongodb
6. echo "extension=mongodb.so" >> /etc/php.d/20-mongodb.ini
7. 驗證 php -m | grep mongodb


## docker 
- https://hub.docker.com/r/jhaoheng/php_env/
- `docker pull jhaoheng/php_env:1.1`