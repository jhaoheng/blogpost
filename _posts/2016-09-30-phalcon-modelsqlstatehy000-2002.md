---
layout: post
title: '【phalcon model】SQLSTATE[HY000] [2002]'
date: 2016-09-30 03:06
comments: true
categories: 
---
## env

- phalcon DevTools (3.0.1)
- MAMP 3.5.2
	- php 7.0.8
	- mysql

## Trouble

- cmd : `phalcon model --name [table_name]`
- error : `ERROR: SQLSTATE[HY000] [2002] No such file or directory`

## Fix

Because the phalcon-DevTools [php version] does not set MAMP mysql source

use
 
- `php -ini`
- `vim php.ini`
- search `mysql.default_socket` 填入 `/Applications/MAMP/tmp/mysql/mysql.sock`
	- `pdo_mysql.default_socket = /Applications/MAMP/tmp/mysql/mysql.sock`
- restart MAMP
