---
layout: post
title: '【ossrs】compile ffmpeg'
date: 2017-05-17 02:14
comments: true
categories: 
---
# env

- osx : 10.11.6

# question

1. `./configure --osx --full --with-ffmpeg`

error : Libtool library uesd but 'LIBTOOL' is undefined

2. fixed `brew install libtool`