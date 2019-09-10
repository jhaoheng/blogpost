---
layout: post
title: '【phalcon】migration'
date: 2016-07-19 02:08
comments: true
categories: 
---
# migration

- phalcon 2.0.13 : https://github.com/phalcon/cphalcon
- Phalcon DevTools (2.0.13) : https://github.com/phalcon/phalcon-devtools

## main cmd

- `phalcon migration --help`
- `phalcon migration generate` : 產生 migration file
	- `migration generate 1.0.1` : 1.0.1 為產生的版本號，預設為 1.0.0
	- 預設路徑為 `app/migrations/`
- `phalcon migration run` : 執行產生的 migration