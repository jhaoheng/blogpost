---
layout: post
title: '【EJABBERD 17.07】dockerfile'
date: 2017-08-17 10:16
comments: true
categories: Ejabberd
---
```
FROM        ubuntu:16.04

EXPOSE      5222 5280 5269 4560

WORKDIR     /home

ENV         DL=https://www.process-one.net/downloads/ejabberd/17.07/ejabberd_17.07-0_amd64.deb
ENV			EJABBERD_HOME_NAME=ejabberd-17.07
ENV         DL_FILENAME=$EJABBERD_HOME_NAME.deb 

RUN         apt-get update
RUN 		apt-get install libexpat1 apt-utils -y
RUN         apt-get install wget vim -y
RUN         wget -O $DL_FILENAME $DL
RUN         dpkg -i $DL_FILENAME
```