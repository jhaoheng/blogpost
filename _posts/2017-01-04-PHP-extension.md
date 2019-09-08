---
layout: post
title: "【php 7.x】/private/tmp/pear/install/yaml/php_yaml.h:56:10: fatal error: 'ext/standard/php_smart_str.h' file not found"
date: 2017-01-04 09:40
comments: true
categories: PHP
---

## install ymal.so

`sudo pecl insatll yaml`
**install Error**
`/private/tmp/pear/install/yaml/php_yaml.h:56:10: fatal error: 'ext/standard/php_smart_str.h' file not found`

## fixed

1. `sudo pecl install yaml-2.0.0`
2. add yaml.so to php.ini extension
	- use MAMP
  	1. mv yaml.so to `/Applications/MAMP/bin/php/php7.0.8/lib/php/extensions/no-debug-non-zts-xxxxxxxxxx`
    2. edit php.ini of php7.0.8
  - others : `echo "extension=yaml.so" > /usr/local/etc/php/conf.d/ext-yaml.ini`