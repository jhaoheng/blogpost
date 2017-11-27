---
layout: post
title: 【GOLANG】viper parse yaml
date: 2017-11-28 00:54
categories: golang yaml
---

# synopsis

1. source : `https://github.com/spf13/viper`
2. 透過 viper 解析 yaml 格式
3. 將 yaml 格式放到 struct 中

<!--more-->

# example

```config.yaml
# database
database: 
  - host: "localhost"
    port: "6612"
    username: "root"
    password: "root"
    dbname: "demo"
```



```main.go
package main

import (
    "fmt"
    "github.com/spf13/viper"
)

type mysql struct {
    Host     string
    Port     string
    Username string
    Password string
    Dbname   string
}

type Config struct {
    Database []mysql
}

func main() {

    viper.SetConfigName("config") // name of config file (without extension)
    viper.AddConfigPath(".")    // optionally look for config in the working directory
    err := viper.ReadInConfig() // Find and read the config file
    if err != nil {             // Handle errors reading the config file
        panic(fmt.Errorf("Fatal error config file: %s \n", err))
    }

    fmt.Println(viper.Get("database"))

    var config Config
    viper.Unmarshal(&config)
    fmt.Println(config)

    var database []mysql
    viper.UnmarshalKey("database", &database)
    fmt.Println(database)
}
```