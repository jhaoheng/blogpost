---
layout: post
title: "【PHP-OSX】mongodb / php-drive / client-lib"
date: 2017-05-09 09:10
comments: true
categories: 
---
# 環境
- osx
- php7.0.8
- mongod 3.3.4
- mongodb : http://php.net/manual/en/set.mongodb.php
  - 勿與 舊版的 Mongo drive 混淆，因為使用上會有極大差別
- php-mongodbClient 1.2.8

# 安裝 mongodb

1. `$ brew install mongodb`
2. 可在 `$ cat /usr/local/etc/mongod.conf`，看到 mongodb 設定
	- 注意位置 /data/db，確定是否存在，若不存在則建立一個，記得打開權限
3. `$ mongod` 啟動 mongodb

# 設定 php drive

> - 有分 mongo.so 與 mongodb.so 版本
- mongodb.so 支援 php7, 用 MongoDB\Driver\xxxxxx 來進行 new 的動作。http://stackoverflow.com/a/38007889
- mongo.so 是用 MongoClient 來進行管理

1. `$ brew install php70-mongodb`
2. `$ cd /usr/local/Cellar/php70-mongodb` => 找到 mongodb.so
3. `$ php -i | grep extension` => 找到 extension 的位置
4. `$ mv mongodb.so {extension 位置}`
5. `$ php --ini` => 找到 php.ini
6. `$ vim php.ini`
	- 在 extension 中，加入 extension=mongodb.so
7. 驗證在 `$ php -i | grep mongo` 驗證版本應該跟下載的相同

## 當執行 mongo 提取資料時出現

```
dyld: lazy symbol binding failed: Symbol not found: _clock_gettime
  Referenced from: /usr/local/opt/php70-mongodb/mongodb.so
  Expected in: /usr/lib/libSystem.B.dylib

dyld: Symbol not found: _clock_gettime
  Referenced from: /usr/local/opt/php70-mongodb/mongodb.so
  Expected in: /usr/lib/libSystem.B.dylib
```
解決方法 : https://github.com/perftools/xhgui/issues/198#issuecomment-286711319

# 設定 MAMP

1. 一樣將 mongodb.so 拷貝到 mamp extension 的位置底下
2. 編輯 mamp 的 php.ini
3. 透過 phpinfo() 來檢查

# php 使用 mongodb drive
- http://php.net/manual/en/class.mongodb-driver-manager.php

### 更有效率地使用 mongodb drive
- https://docs.mongodb.com/php-library/master/tutorial/install-php-library/
- MongoDB client-libraby github : https://github.com/mongodb/mongo-php-library
- 搜尋的碼
```php
$client = new MongoDB\Client;
$collection = $client->selectCollection('db', 'collection');
$cursor = $collection->find();
foreach ($cursor as $restaurant) {
   var_dump($restaurant);
};
```