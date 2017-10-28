---
layout: post
title: "【phalcon】install 'cphalcon', 'phalcon-tool' by brew & mamp"
date: 2016-08-04 10:07
comments: true
categories: 
---
## brew install

- env 
  - homebrew 0.9.9
  - php 7.0.9
  - phalcon 3.0.0

```
brew install php70
brew install php70-phalcon
```
## check 

- `php -m | grep -i phalcon` -> should output `phalcon`
- cmd `phalcon` -> should output phalcon cmd info, include phalcon version

## php\_info()

```
<?
php\_info();
?>
```
## mamp install phalcon

- env 
  - mamp : 3.5.2
  - php 7.0.8
  - phalcon 3.0.0
  
> use brew's `php70-phalcon/phalcon.so`
move to `/Applications/MAMP/bin/php/php7.0.8/lib/php/extensions/no-debug-non-zts-xxxxxxxxxxx/`

