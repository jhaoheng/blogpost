---
layout: post
title: '【php】memcached'
date: 2017-01-15 15:24
comments: true
categories: PHP
---
# ENV
- php 7.x 
- mac osx

# 編譯好的 memcache.so
- github : https://github.com/majksner/php-memcached-mamp

1. 下載 memcached.so
2. brew install memcached
3. brew install libmemcached
4. php --ini
5. vim php.ini
6. extension=memcached.so
7. php -i | grep memcached


# 自己 compile

- git clone https://github.com/php-memcached-dev/php-memcached.git

1. brew install memcached
2. brew install libmemcached
3. git checkout php7
4. phpize
5. `./configure --with-memcached=/usr/local/Cellar/libmemcached/1.0.18_1/lib/libmemcached`
	- Notice : the `libmemcached` path is brew install path
6. make
7. sudo make install
8. mv memcached.so to extension path, you could get path with `php -i | grep extension_dir`
9. vim php.ini, add `extension=memcached.so`


# 如何使用

## 前提
1. 安裝好 memcached 在電腦中 : brew install memcached
	- 至於 libmemcached 則為編譯成 .so 時使用
2. 執行 memcached 即可
	- `man memcached`
  - `memcached -l 127.0.0.1 -P 11211 -m 128 -d` : for deamon
  - `memcached -l 127.0.0.1 -P 11211 -m 128 -vv` : for debug
3. 檢查 memcached 是否有執行 : `ps aux | grep memcached`

## 判斷 php 是否安裝好支援的 memcached.so

php -i | grep memcached
php -m | grep memcached

## test
```
<? php
$mem  = new Memcached();

$mem->addServer('localhost',11211);
if( $mem->add("key","value",3600)){
    echo  'cached!';
}else{
    echo 'cached data：'.$mem->get("key");
}
?>
```

## console admin

`git@github.com:jhaoheng/phpmemcacheadmin.git`