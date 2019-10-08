---
layout: post
title: 【Go】模板語言 & docker inspect
date: 2019-10-08 17:06
categories: Golang
tags: Golang
---

# golang 模板語言
- Resource : https://golang.org/pkg/text/template/

#  在 docker inspect 中的應用
>https://docs.docker.com/engine/reference/commandline/inspect/

- 通常會使用 
	- `docker inspect -f '{...}' {container_id}`
	- `docker network inspect -f '{...}' {network_name}`
- 兩者在 inspect 的使用上都相同
- 其中 {...} 就是使用模板語言進行互動

# example : docker-compose.yml
```
version: "3.7"
services: 
  s1:
    image: nginx:1.15
    container_name: s1
    ports:
      - 81:80
    networks:
      - sample
  s2:
    image: nginx:1.15
    container_name: s2
    ports:
      - 82:80
    networks:
      - sample
  s3:
    image: nginx:1.15
    container_name: s3
    ports:
      - 83:80
    networks:
      - sample

networks:
  sample:
    name: sample-net
```

<!--more-->

# 範例資料
> 以下是 network 的部分資訊, name = sample-net

```
[
    {
        "Name": "sample-net",
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "192.168.96.0/20",
                    "Gateway": "192.168.96.1"
                }
            ]
        },
        "Containers": {
            "02031a4daf350456c039b6cf42b41407757a662c8c25def11b9feed8f3b9b973": {
                "Name": "s1",
                "EndpointID": "2db733478ebf138e3f770be27a2195256ed63204784476e64d05ad3a400b97fe",
                "MacAddress": "02:42:c0:a8:60:02",
                "IPv4Address": "192.168.96.2/20",
                "IPv6Address": ""
            }
        },
    }
]
```

# filter format

## 變量 : 獲取 "Name"
`docker network inspect -f '{{.Name}}' sample-net`

## 變量 : 獲取 "IPAM"
`docker network inspect -f '{{.IPAM}}' spider-net`

## 變量 : 獲取 .IPAM.Driver
`docker network inspect -f '{{.IPAM.Driver}}' sample-net`

## 索引 : 獲取 .IPAM.Config [0] 的 Subnet
- 索引 
	- 如果返回結果是一個 map, slice, array 或 string，則可以使用 index 加索引序號（從零開始計數）來讀取屬性值。
- `docker network inspect -f '{{(index .IPAM.Config 0).Subnet}}' sample-net`

## 遍例 : 獲取 Containers 中的所有名稱
- `{{range Object}}{{.}}{{end}}`, 支持的類型包括 array, slice, map 和 channel
	-  對應的值長度為 0 時，range 不會執行。
	-  結構內部如要使用外部的變量，需要在前面加 引用，比如 Var2。
	-  range 也支持 else 操作。效果是：當返回值為空或長度為 0 時執行 else 內的內容。
- ex:
	- `docker network inspect -f '{{range .Containers}}{{.Name}}{{end}}' spider-net`
	- `docker network inspect -f '{{range .Containers}}{{.Name}}{{println}}{{end}}' sample-net`
	- `docker network inspect -f '{{range .Containers}}{{.Name}}{{println}}{{else}}With No Containers{{end}}' sample-net`

## if ... else ... end

- General : `docker network inspect -f '{{if .Name}}hello world{{end}}' sample-net`
- Not : `docker network inspect -f '{{if not .Name}}No Name{{else}}Name exist{{end}}' sample-net`
- Or :
	-   {{or x y}}: 表示如果 x 為真返回 x，否則返回 y。
	-   {{or x y z}}：後面跟多個參數時會逐一判斷每個參數，並返回第一個非空的參數。如果都為 false，則返回最後一個參數。
	-  除了 null（空）和 false 被識別為 false，其它值（字符串、數字、對象等）均被識別為 true。

## if 判斷式
- eq : Returns the boolean truth of arg1 == arg2
- ne : Returns the boolean truth of arg1 != arg2
- lt : Returns the boolean truth of arg1 < arg2
- le : Returns the boolean truth of arg1 <= arg2
- gt : Returns the boolean truth of arg1 > arg2
- ge : Returns the boolean truth of arg1 >= arg2

- ex : 
	- 輸出所有已停止容器的名稱 : 
		- `docker inspect --format '{{if ne 0.0 .State.ExitCode}}{{.Name}}{{end}}' $(docker ps -aq)`
		- `docker inspect --format '{{if ne 0.0 .State.ExitCode}}{{.Name}}{{else}}容器還在運行{{end}}' $(docker ps -aq)`
		- `docker inspect --format '{{if ne 0.0 .State.ExitCode}}{{.Name}}{{else if .}}容器還在運行{{end}}' $(docker ps -aq)`

## Print
- print 	: 將傳入的對象轉換為字符串並寫入到標準輸出中。如果後跟多個參數，輸出結果之間會自動填充空格進行分隔。
- println 	: 功能和 print 類似，但會在結尾添加一個換行符。也可以直接使用 {{println}} 來換行。
- printf 	: 與 shell 等環境一致，可配合佔位符用於格式化輸出。

## 取得長度
- `docker inspect --format '{{len .Name}}' sample-net`

## 輸出用 json 方式取代 text
- `docker inspect --format '{{json .IPAM}}' sample-net`

## join
- `docker inspect -f '{{join .HostConfig.MaskedPaths ", "}}' s1`

## title && lower && upper

- lower : 全部轉為小寫
	- `docker network inspect --format '{{range .Containers}}{{lower .Name}}{{end}}' sample-net`
- upper : 全部轉為大寫
	- `docker network inspect --format '{{range .Containers}}{{upper .Name}}{{end}}' sample-net`
- title : 將返回的結果值，首字母轉為大寫
	- `docker network inspect --format '{{range .Containers}}{{title .Name}}{{end}}' sample-net`

## split
- `docker network inspect --format '{{range .Containers}}{{split .Name "/"}}{{end}}' sample-net`
	- output : `[s1][s2][s3]`
