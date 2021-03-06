---
layout: post
title: "【AWS-S3】多檔案上傳"
date: 2017-06-07 16:45
comments: true
categories: 
---
# env
- aws sdk php v3
- php 7.0x

# ex

```
require './vendor/autoload.php';
use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\CommandPool;

$access_key = "";
$secret_access_key = "";
$bucket = "";

$client = S3Client::factory([
        "region" => "ap-northeast-1", // tokyo
        "version" => "latest", 
        'credentials' => [
            'key'    => $access_key,
            'secret' => $secret_access_key,
        ],
    ]
);

$commands = array();

$binary = file_get_contents('./images/1.jpg');
$commands[] = $client->getCommand('PutObject', [
        'Bucket' => $bucket,
        'Key'    => 'photos/photo01.jpg',
        'Body'   => $binary
    ]
);

$commands[] = $client->getCommand('PutObject', [
        'Bucket' => $bucket,
        'Key'    => 'photos/photo02.jpg',
        'Body'   => $binary
    ]
);

$pool = new CommandPool($client, $commands);
// Initiate the pool transfers
$promise = $pool->promise();

// Force the pool to complete synchronously
try {
    $result = $promise->wait();
} catch (AwsException $e) {
    // handle the error.
}
```