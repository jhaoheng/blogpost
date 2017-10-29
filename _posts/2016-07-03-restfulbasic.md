---
layout: post
title: '【RESTFUL】basic'
date: 2016-07-03 23:56
comments: true
categories: 
---
# restful

- [source](http://www.restapitutorial.com/lessons/httpmethods.html)

## Six constraints

- Uniform Interface
- Stateless
- Cacheable
- Client-Server
- Layered System
- Code on Demand

## HTTP Verbs

- Use HTTP Verbs to Mean Something
	- GET / POST / PUT / PATCH / DELETE
- Sensible Resource Names
	- Resource names should be nouns—avoid verbs as resource names. It makes things more clear. Use the HTTP methods to specify the verb portion of the request.

ps: 這些動詞，在 http 中，有某些特別的屬性，若不遵循這些規則，一樣可以使用，但只是與某些淺規則相悖

### PUT

- Update/Replace : 新增一項資料，如果存在就覆蓋過去。（還是只有一筆資料，故與 `POST` 有所差別）
- ex : `/customers` -> 404 (Not Found)
- ex : `/customers/{id}` -> 200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid.

### POST

- Create : 新增一項資料。（如果存在會新增一個新的）
- ex : `/customers` -> 404 (Not Found). 如果預計 POST 批次的話，或許可以考慮
- ex : `/customers/{id}` -> 201 (Created).

ps : 用 [GET] : `/customers/{id}` -> 取得用戶資訊

### PATCH

- Update/Modify : 附加新的資料在已經存在的資料後面。（資料必須已經存在，patch會擴充這項資料）

### DELETE

- Delete : 刪除資料。
- ex : `/customers/` -> 404 (Not Found).
- ex : `/customers/{id}` -> 200 (OK). 404 (Not Found), if ID not found or invalid.

### GET 

- Read : 讀取資料。
- ex : `/customers` -> 200 (OK), list of customers.
- ex : `/customers/{id}` -> 200 (OK), single customer. 404 (Not Found), if ID not found or invalid.

### HEAD

- 取得 get 的 http header 而不取得內容

## 關於 safe 與 idempotent 

- safe : 是否安全(可以快取)。在於『定義上』是否屬於會修改資料的 method，如果『會』代表就是不安全(POST/PUT/PATCH/DELETE 都不可以快取)。
- idempotent : 頁面重新整理是否會影響 request，也就是該 method 在重新整理後，是否會重送（是否可以在不確定有沒有成功送出時重新發出請求），相同的 Request 再執行一次，結果還是一樣。
	- 在不同的情況下運用


|        | safe | idempotent |
|:------:|:----:|:----------:|
| PUT    |  [ ] |     [x]    |   
| POST   |  [ ] |     [ ]    |
| PATCH  |  [ ] |     [ ]    | 
| DELETE |  [ ] |     [x]    |
| GET    |  [x] |     [x]    |
| HEAD   |  [X] |     [x]    |

## 關於狀態回傳

- http status code
- status : success or failure
- message : Json to describe some about response.
