---
layout: post
title: '【MAMP】install phalcon'
date: 2016-07-04 01:56
comments: true
categories: 
---
# MAC/MAMP install phalcon

- [source](https://github.com/majksner/php-phalcon-mamp)

## Install on MAMP

### phalcon.so version

- php5.4.34
- php5.5.26
- php5.6.7
- php5.6.10

### step

1. Open MAMP
2. PHP -> default version 
3. download suitable `phalcon.so` from [github](https://github.com/majksner/php-phalcon-mamp)
4. `cd /Applications/MAMP/bin/php/php5.x.xx` and `lib/php/extensions/` and 'no-debug-non-zts-xxxxxxxx'
5. cp `phalcon.so` to (4) path
6. open MAMP navigation bar，File -> Edit template -> PHP -> (choose currect php version which want to use)
7. Add `extension=phalcon.so` of php.ini
8. restart MAMP 