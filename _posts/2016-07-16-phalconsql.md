---
layout: post
title: '【phalcon】sql'
date: 2016-07-16 15:44
comments: true
categories: 
---
# 使用 phql + Avoid sql injection

## select

```
$conditions = "Users.id = :id:";
$parameters = array(
    "aid" => '31'
);

$phql = "SELECT b.* FROM Users as a RIGHT JOIN Devices as b on a.id=:aid:";
$result = $app->modelsManager->executeQuery($phql,$parameters);
```
- 其中的 `$app` 為 Micro 建立的物件
- `modelsManager` 為 model 的 method

## insert

```
$parameters = array(
    "key" => "123",
    "user_id" => 3,
);

$phql = "INSERT INTO tableName (key, user_id) VALUES (:key:, :user_id:)";
$result = $app->modelsManager->executeQuery($phql, $parameters);

if ($result==true) {
	echo "success\n\r";
}
```

# 使用 抽象層 建立 sql 語法，記錄 log

- 因若透過抽象層建立 sql，很難直接看出 sql 是否正確，可透過 phalcon 建立 log，並將 db 的所有動作記錄

```
use Phalcon\Mvc\Model\Criteria;
use Phalcon\Logger;
use Phalcon\Logger\Adapter\File as FileLogger;
use Phalcon\Events\Manager;

$logger = new FileLogger("logs/debug.log");
$eventsManager = new Manager();
$connection = $app['db'];
$eventsManager->attach('db', function ($event, $connection) use ($logger) {
        if ($event->getType() == 'beforeQuery') {
			$logger->log($connection->getSQLStatement(), Logger::INFO);
        }
});
$app['db']->setEventsManager($eventsManager);// 儲存所有 db 動作

$conditions = "Users.id = :id:";
$parameters = array(
    "id" => '31'
);

$result = $app->modelsManager->createBuilder()
	->columns(array('Devices.*'))
	->from('Devices')
	->rightJoin('Users')
	->where($conditions,$parameters)
	->getQuery()
	->execute();
```