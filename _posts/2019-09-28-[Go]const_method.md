---
layout: post
title: 【Go】Type with string, and Method
date: 2019-09-28 21:17
categories: Golang
tags: Golang
---

# Thinking

- 通常比較常看到的是，命名一個 struct 類型，然後為這個 struct 實踐方法。
- 今天看別人的 code 發現有這種做法，一開始還在想，為何要特別這樣做，才覺得這是 clean code 的一種方式。

## Glance code

```
package main

import (
	"errors"
	"fmt"
	"time"
)

type Country string

const (
	Germany      Country = "Germany"
	UnitedStates Country = "United States"
	NewZealand   Country = "New Zealand"
)

// timeZoneID is a map of Country to its IANA standard timezone identifier
var timeZoneID = map[Country]string{
	Germany:      "Europe/Berlin",
	UnitedStates: "America/Los_Angeles",
	NewZealand:   "Pacific/Auckland",
}

// TimeZoneID returns a IANA identifier for a given Country.
func (c Country) TimeZoneID() (string, error) {
	if id, ok := timeZoneID[c]; ok {
		return id, nil
	}
	return "", errors.New("timezone id not found for country")
}

// TimeIn returns time in timezone tz with fmt format
func TimeIn(t time.Time, tz, fmt string) string {

	// https:/golang.org/pkg/time/#LoadLocation loads location on
	// the basis of
	loc, err := time.LoadLocation(tz)
	if err != nil {
		//handle error
	}

	// convert current time to specific location, e.g Germany in given format
	return t.In(loc).Format(fmt)
}

func main() {
	// Get the timezone
	tz, err := UnitedStates.TimeZoneID()
	if err != nil {
		//handle error
	}

	usTime := TimeIn(time.Now(), tz, time.RFC3339)

	fmt.Printf("Time in %s: %s",
		UnitedStates,
		usTime,
	)
}
```

- 在這邊設定一個新的型態 Country, 目的是設定 只接收該型態的 method.
	- 若直接使用 string, 則無法直接使用 以 string 為形態的 method.
- 從 main() 來看，可以清楚的知道，物件 `UnitedStates` 的 TimeZoneID
	- 不直接命名的方式，也可以幫助後續無論是 Test or Debug，一種延續系統開發的作法


> 至於 RFC3339 是一種時間的顯示格式, 通常選擇格式, 能幫助我們在寫入資料庫選擇 `DATETIME` or `TIMESTAMP` 的格式選擇 'YYYY-MM-DD hh:mm:ss'

```
const (
    ANSIC       = "Mon Jan _2 15:04:05 2006"
    UnixDate    = "Mon Jan _2 15:04:05 MST 2006"
    RubyDate    = "Mon Jan 02 15:04:05 -0700 2006"
    RFC822      = "02 Jan 06 15:04 MST"
    RFC822Z     = "02 Jan 06 15:04 -0700" // RFC822 with numeric zone
    RFC850      = "Monday, 02-Jan-06 15:04:05 MST"
    RFC1123     = "Mon, 02 Jan 2006 15:04:05 MST"
    RFC1123Z    = "Mon, 02 Jan 2006 15:04:05 -0700" // RFC1123 with numeric zone
    RFC3339     = "2006-01-02T15:04:05Z07:00"
    RFC3339Nano = "2006-01-02T15:04:05.999999999Z07:00"
    Kitchen     = "3:04PM"
    // Handy time stamps.
    Stamp      = "Jan _2 15:04:05"
    StampMilli = "Jan _2 15:04:05.000"
    StampMicro = "Jan _2 15:04:05.000000"
    StampNano  = "Jan _2 15:04:05.000000000"
)
```