--- 
layout: post
title: "【Docker:mariadb】set init when first running"
date: 2017-11-03 12:01
categories: docker mairadb
---
# 目標

1. 為了讓每次測試更加快速的建立 db 中的資料環境，分開 schema.sql & init_value.sql 兩個檔案，方便修改
2. 步驟
	1. 透過 docker-compose 建立 container:mariadb
	2. 透過 volume 的方式，掛載 base_schema.sql & init_value.sql 檔案
	3. 建立的過程中，讓 container:mariadb 自行初始化 (2) 的檔案
<!--more-->
# docker

- hub : `https://hub.docker.com/_/mariadb/`

> ## Initializing a fresh instance
When a container is started for the first time, a new database with the specified name will be created and initialized with the provided configuration variables. Furthermore, it will execute files with extensions .sh, .sql and .sql.gz that are found in /docker-entrypoint-initdb.d. Files will be executed in alphabetical order. You can easily populate your mariadb services by mounting a SQL dump into that directory and provide custom images with contributed data. SQL files will be imported by default to the database specified by the MYSQL_DATABASE variable.

## 測試

1. sqlfile 檔案名稱，不能用數字代替英文字母
2. 所有的檔案要放在 container 中的 `/docker-entrypoint-initdb.d` 位置

## sql file

- schema

```
CREATE DATABASE IF NOT EXISTS `test` DEFAULT CHARACTER SET latin1 DEFAULT COLLATE latin1_swedish_ci;
USE `test`;
SET @PREVIOUS_FOREIGN_KEY_CHECKS = @@FOREIGN_KEY_CHECKS;
SET FOREIGN_KEY_CHECKS = 0;

CREATE TABLE `demo` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  PRIMARY KEY (`id`),
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

SET FOREIGN_KEY_CHECKS = @PREVIOUS_FOREIGN_KEY_CHECKS;
```

- init value

```
CREATE DATABASE IF NOT EXISTS `test` DEFAULT CHARACTER SET latin1 DEFAULT COLLATE latin1_swedish_ci;
USE `test`;
SET @PREVIOUS_FOREIGN_KEY_CHECKS = @@FOREIGN_KEY_CHECKS;
SET FOREIGN_KEY_CHECKS = 0;

LOCK TABLES `demo` WRITE;
ALTER TABLE `demo` DISABLE KEYS;
INSERT INTO `demo` (`id`, `name`) VALUES (1,'maxhu');
ALTER TABLE `demo` ENABLE KEYS;
UNLOCK TABLES;

SET FOREIGN_KEY_CHECKS = @PREVIOUS_FOREIGN_KEY_CHECKS;
```

## docker-compose

```
version: '3.2'
services:
  mariadb:
    image: mariadb
    container_name: mariadb
    ports:
      - 3306:3306/tcp
    environment:
      - MYSQL_ROOT_PASSWORD=root
    volumes:
      # 依照 字母順序 依序執行，不能用數字
      - ./databse/schema.sql:/docker-entrypoint-initdb.d/base_schema.sql
      - ./databse/init_values.sql:/docker-entrypoint-initdb.d/init_values.sql
```