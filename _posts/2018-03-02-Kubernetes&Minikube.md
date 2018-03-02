---
layout: post
title: 【Deploy Test】Use Kubernetes & Minikube(local)
date: 2018-03-02 07:30
categories: kubenetes minikube
---

## Synopsis
- 透過本地端，練習 kubenetes client 
- 使用 minikube&virtual 當作 kube server
- minikube 本身提供像 gcloud 一樣 GUI 介面，方便具有既視感
	- docker 本身提供的 kubernetes server 目前沒有 GUI 介面

## Environment
- osx
- minikube & virtualbox
- kubernetes client

<!--more-->

## Install
1. 安裝 [virtual box](https://www.virtualbox.org/wiki/Downloads)
	- 此次安裝的版本為 5.2.8
	- mac 安裝失敗問題 : http://blog.csdn.net/u013247765/article/details/78176079
2. osx 安裝 minikube : `brew cask install minikube`
	- minikube start
	- 若遇到失敗 : `minikube delete`
	- [操作方法](https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/)
	- [Github](https://github.com/kubernetes/minikube)
	- 開啟 web dashboard : `minikube dashboard`
3. 安裝 kubernetes client [官網安裝](https://kubernetes.io/docs/imported/release/notes/)
	1. 進入安裝頁面
	2. 選擇 client binaries
	3. 確定自己電腦的 linux 安裝即可 : osx amd64 
		- `uname -a` : 看是否是 x86_64
		- `shasum -a 256 {the file} | grep {hash}` : 在 osx 驗證 hash
	4. 放入 bash_profile, `source ~/.bash_profile` 即可