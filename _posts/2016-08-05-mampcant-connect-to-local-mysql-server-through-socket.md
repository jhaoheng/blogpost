---
layout: post
title: "【MAMP】Can't connect to local mySql server through socket"
date: 2016-08-05 02:57
comments: true
categories: 
---
# MAMP
- 3.5.2

## Situation
Use third app to access MAMP mysql : [Querious]
(In general, you can use phpMyAdmin.....)

# troubleshoot

## Can't connect to local mySql server through socket /tmp/mysql.sock

```
ln -s /Application/MAMP/tmp/mysql/mysql.sock /tmp/mysql.sock
```