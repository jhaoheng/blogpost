---
layout: post
title: 'Dynamic Programming'
date: 2016-05-31 16:18
comments: true
categories: 
---
# 動態規劃(Dynamic Programming)

[Line 面試](http://wangyung.blogspot.tw/2016/05/blog-post.html?utm_content=bufferd71fd&utm_medium=social&utm_source=facebook.com&utm_campaign=buffer)
中有一題，提到『動態規劃』一詞與 Fibonacci 的設計方法

稍微 review 一下，『為何 Fibonacci 中單純用遞迴，效率不高』
透過『動態規劃』，把每一次計算出的結果 儲存起來，之後重複的運算，就取出之前暫存的(cached)結果即可

感覺有點類似之前在做數據分析時，在 cached 相同，或者相似資料時，所做的處理
避免重複性的計算，增加程式的效率

Dynamic Programming Approaches:
- Bottom-Up
- Top-Down

[參考資料](http://algorithms.tutorialhorizon.com/introduction-to-dynamic-programming-fibonacci-series/)