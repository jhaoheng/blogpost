# 新增文章需注意

```
---
layout: post
title: 【Go】Drill interface{}
date: 2019-09-07 16:17
categories: Golang
tags: Golang
---
```

- 注意 日期 只能是 : `yy-mm-dd hh:mm`
- 請將文章放在 `_posts` 中

## embedded go playground
```
<div>
    <iframe src="https://play.golang.org/p/WLS7doOuphz" height="315" width="560" allowfullscreen="" frameborder="1">
    </iframe>
</div>
```

## 文章截斷
`<!--more-->`

## 文章連結
- `{{ site.url }}` : 網站 url
- `{{ site.url }}{% link _posts/xxxxx.md %}` : 連接 markdown 文章
- `{{ site.url }}{% link /assets/xxx.pdf %}` : 連接 pdf


# localhost test
- localhost : `bundle exec jekyll server -b ''`
- 因 theme 在 baseurl 的 localhost 與 gh-pages 在 deploy 會產生衝突，故 config 設定的 baseurl 以 deploy 為主

# deploy
- build : `bundle exec jekyll build`
- upload : `git add . && git commit -m "update" && git push origin master`

# 環境設定
- `./config.yml`