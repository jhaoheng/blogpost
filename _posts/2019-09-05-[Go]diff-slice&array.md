---
layout: post
title: 【Golang】Diff - slice & array
date: 2019-09-06 14:31
categories: Golang
---

# slice & array 差別
> https://blog.golang.org/go-slices-usage-and-internals

# Array
```
Go's arrays are values. An array variable denotes the entire array; it is not a pointer to the first array element (as would be the case in C). This means that when you assign or pass around an array value you will make a copy of its contents. (To avoid the copy you could pass a pointer to the array, but then that's a pointer to an array, not an array.) One way to think about arrays is as a sort of struct but with indexed rather than named fields: a fixed-size composite value.
- An array's size is fixed;
- Arrays do not need to be initialized
```

# Slice
```
The type specification for a slice is []T, where T is the type of the elements of the slice. Unlike an array type, a slice type has no specified length.
A slice can be created with the built-in function called make, which has the signature,
`func make([]T, len, cap) []T`
```

# Glance : Difference between both

- 兩者在宣告方式不同
- 兩者 type 不同
- 兩者 kind 不同
- 兩者在宣告後, 不設定 content, 印出內容
- 兩者在新增/修改值上不同
- 兩者 func 中，使用原理方式不同

<!--more-->

## 兩者在宣告方式不同 : 
- array : Go's arrays are values.
	- `var arrObject [4]int`
	- `arrObject := [4]int{0, 1, 2, 3}`
	- `arrObject := [...]int{0, 1, 2, 3}`
- slice : 用 make 建立
	- `var sliceObject []int`
	- `sliceObject := []int{}`
	- `sliceObject := []int{1,2,3,4}`

## 兩者 type 不同
- array : `fmt.Println(reflect.TypeOf(arrObject))`, 顯示 [4]int
- slice : `fmt.Println(reflect.TypeOf(sliceObject))`, 顯示 []int

<div>
    <iframe src="https://play.golang.org/p/WLS7doOuphz" height="315" width="560" allowfullscreen="" frameborder="1">
    </iframe>
</div>

## 兩者 kind 不同 : 
- array : `fmt.Println(reflect.TypeOf(arrObject).Kind())`, 顯示 array
- slice : `fmt.Println(reflect.TypeOf(sliceObject).Kind())`, 顯示 slice

<div>
    <iframe src="https://play.golang.org/p/6BD2qUVm00l" height="315" width="560" allowfullscreen="" frameborder="1">
    </iframe>
</div>
	
## 兩者在宣告後, 不設定 content, 印出內容

- array : An array's size is fixed
- slice : A slice type has no specified length.

<div>
    <iframe src="https://play.golang.org/p/xXnbKIC5WhV" height="315" width="560" allowfullscreen="" frameborder="1">
    </iframe>
</div>

## 兩者在新增/修改值上不同
- array 
	- arr 長度固定, 無法新增
	- 新增、修改 : `arrObject[0] = 999`
- slice
	- 新增 : `sliceObject = append(sliceObject, 1,2,3,4)`
	- 修改 : `sliceObject[0] = 999`

<div>
    <iframe src="https://play.golang.org/p/HY7kVBdV-KZ" height="315" width="560" allowfullscreen="" frameborder="1">
    </iframe>
</div>

## 兩者 func 中，使用原理方式不同
> 範例中用到的 func, 需要注意 pointer

- array 在 func 中, 是用 copy 的方式. 故最好用 pointer 的方式在傳遞 array
- slice 本身就是一個 pointer, 故可以在 func 中, 看到 address 相同

<div>
    <iframe src="https://play.golang.org/p/Tt8rJNMjDAm" height="315" width="560" allowfullscreen="" frameborder="1">
    </iframe>
</div>

# The same

## 兩者在切割方式上**相同** : 
- `arr[:2]` = `sliceObject[:2]`

## 顯示長度上
- array : `fmt.Println(len(arrayObject))`, 顯示 4
- slice : `fmt.Println(len(sliceObject))` 顯示 0