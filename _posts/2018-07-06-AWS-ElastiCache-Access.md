---
layout: post
title: 【AWS-Redis】How to Access to AWS-ElastiCache
date: 2018-07-06 07:30
categories: aws redis
---

# 透過 redis-cli 連線 elasticache

## 限制
1. 必須透過 aws ec2 進行連線 : 
	- From AWS FAQ: (http://aws.amazon.com/elasticache/faqs/)
Please note that IP-range based access control is currently not enabled for Cache Clusters. All clients to a Cache Cluster must be within the EC2 network, and authorized via security groups as described above.
2. 確定 elasticache 建立時的
	- endpoint
	- Subnet Group
	- Security Group 
3. 確定 aws ec2 
	- 確保 instance 的 vpc 與 elasticache 的 Subnet Group 相同

<!--more-->

## 安裝 redis-cli, 連線
1. 先 access 進 ec2 instance
2. `apt-get update && apt-get install gcc make -y`
3. `wget http://download.redis.io/redis-stable.tar.gz && tar xvzf redis-stable.tar.gz && cd redis-stable && make distclean && make`
4. (optional) `ln -s $(pwd)/src/redis-cli /bin/redis-cli`
5. connect : 
	- if (3) `redis-cli -c -h {endpoint} -p 6379`
	- `./src/redis-cli -c -h {endpoint} -p 6379`

## 透過 GUI 訪問
1. 先 access 進 ec2 instance
2. `apt-get update && apt-get install nodejs npm -y && ln -s $(which nodejs) /usr/bin/node && npm install -g redis-commander`
3. 執行服務 : `redis-commander --redis-host <elasticache endpoint>`
4. 確定 ec2 security 8081 port 可讓你目前的 ip 進行連線
5. 瀏覽器輸入 : <ec2-instance-ip>:8081
