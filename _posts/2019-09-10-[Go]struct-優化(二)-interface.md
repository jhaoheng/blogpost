---
layout: post
title: 【Go】利用 struct 優化（二）:interface
date: 2019-09-10 19:11
categories: Golang
tags: Golang
---

# Sypnosis

[【Go】利用 struct 優化（一）]({{ site.url }}{% link _posts/2019-09-10-[Go]struct 優化.md %})後，可以得知

- 使用 struct 讓物件統一，更容易理解
- 使用 pointer 來提高 memory 的使用效率
- 使用 func as method 讓程式碼更容易被閱讀 

以下為相關程式碼

<div>
    <iframe src="https://play.golang.org/p/D0_Jvfq5da8" height="315" width="560" allowfullscreen="" frameborder="1">
    </iframe>
</div>


接下來的問題是
1. 兩邊都有相同的 method, read(), 如何建立一個統一的接口, 再執行某 func, for ex:
	- `db read` : 不同的 db 都會有 read 的 method
	- `sign in` : 不同的登入方法, 但可能是 jwt, facebook, email 等
	- `notification` : 不同的 notification 進行設計 (ios,android,aws,極光...)
2. 接續 1, 在實踐上, 再把 func 變成 method, 讓可讀性變高

<!--more-->

## 1. 發現兩者都有 area() 的 method
1. 在程式中, 我們可以用 interface{} 來將兩者的 method 包在一起, 以方便一起處理某些事情
2. 範例程式碼

### 1-1 定義 interface
- 在 struct 中, 定義 fields
- 在 interface 中, 定義 `method set`.

```
type Shape interface {
	area() float64
}
```

- 定義好 interface, 接下來就是透過 func 來實踐『特定功能』, 這邊舉一個簡單的例子 `showArea()`
	- 其他例子的使用，例如 : 發送 notification 後，驗證錯誤訊息
- 在 func 中使用 interfcae type 當作參數送給 function

```
func showArea(shapes ...Shape) {
	for _, s := range shapes {
		fmt.Println(s.area())
	}
}
// main
func main() {
	// init
	c := Circle{5}
	r := Rectangle{Coordinate{0, 0}, Coordinate{10, 10}}

	// Show
	showArea(&c, &r)
}
```

### 1-2 範例程式碼
<div>
    <iframe src="https://play.golang.org/p/e8AgZllGKwB" height="315" width="560" allowfullscreen="" frameborder="1">
    </iframe>
</div>


## 2. 將 func 定義成 method

1. 定義一個新的類型 `type MultiShape struct{}`, 負責存放所有的 shapes
2. 將 func -> method 
3. 範例程式碼


### 2-1 定義 MultiShape

```
type MultiShape struct {
	shapes []Shape
}
```

### 2-2 接下來將 func 改成 method, 並宣告物件 m

```
func (m *MultiShape) showArea() {
	for _, s := range m.shapes {
		fmt.Println(s.area())
	}
}

// main
func main() {
	// init
	c := Circle{5}
	r := Rectangle{Coordinate{0, 0}, Coordinate{10, 10}}

	// Show
	m := MultiShape{
		[]Shape{&c, &r},
	}
	m.showArea()
}
```

### 2-3 範例程式碼
> 額外添加添加 name()

<div>
    <iframe src="https://play.golang.org/p/wD9xvycXuOc" height="315" width="560" allowfullscreen="" frameborder="1">
    </iframe>
</div>


# 延伸問題

- 如何添加周長?
