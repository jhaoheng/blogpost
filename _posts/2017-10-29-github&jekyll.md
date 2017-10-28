---
layout: post
title: "【Github&jekyll】設定"
date: 2017-10-29 02:25
categories:
---
# First
> 從 2009 年開始，從 blogger -> 其他的服務 -> HEXO -> blogger -> logdown，
> 從浪費時間，到專注核心（寫文、po 文），決定了託管(server+頁面的管理)會比較輕鬆，最後選擇了 logdown
> 沒想到 logdown 也會有服務出問題，甚至讓我覺得可能會中斷服務，讓我覺得挺可惜
> 最後還是又回到了 github 中，不過這次用的服務是 `jekyll`

- 環境 : osx
- index
	- 安裝 jekyll
	- 建立 github blog repo
	- 在 repo 中，初始化 jekyll
		- 資料夾結構
	- 為了 github 所調整的設定 (avoid css no render)
		- _config.yml
		- branch : master / gh-pages 
	- 版本控管與測試
	- Note_1 : 更新 osx ruby
	- Note_2 : 建立草稿與預覽

# 安裝 jekyll

> 若 ruby 版本須更新，請參考下方 [Note_1 : 更新 osx ruby]
> 官方 https://jekyllrb.com/

- gem install jekyll

# 建立 github blog repo
1. create a github repo
2. git clone repo

# 在 repo 中，初始化 jekyll
```
cd {your-repo}
jekyll new . # 建立一個新專案
bundle exec jekyll serve
```

在啟動 service 的過程中，發生問題
> kernel_require.rb:55:in `require': cannot load such file -- bundler (LoadError)

解決方法
```
gem install bundler
bundle install
```

## jekyll 資料夾結構

- 官方 : https://jekyllrb.com/docs/structure/
- 第一次建立 jekyll 時，某些資料夾並不會一起建立，需要手動建立
	- _drafts : 請參考 [Note_2 : 建立草稿與預覽]
	- _layouts
	- _sass
	- _data
	- _includes

# 為了 github 所調整的基本設定

## _config.yml
> 目的是為了讓正式版版本，推送到 github 上時，可以找到正確的相關檔案路徑

- baseurl : 請設定為你的 repo 專案名稱
- url : 請設定 `https://{username}.github.io`
- 其他設定對第一次使用部會影響太多

## branch : master / gh-pages 

> master : 負責原始的文件檔
> gh-pages : html 靜態頁面，就是 `_site/` 的資料夾
> 如何分開兩個 branch 與設定，以下說明如何操作

1. 當使用 `jekyll` 產生新的樣板時，在 `.gitignore` 中，會發現 `_site/` 已經被自動忽略
2. 設定好 `_config.yml` 後，執行 `jekyll build` 會建立 `_site/` 此為正式要推送到 gp-pages 的版本，`cd _site`
3. 在 `_site` 中執行 `git init`，新增 `git remote add origin {your_blog_repo}`
4. `git checkout -b gh-pages`
5. `git add .` -> `git commit -m "jekyll first build"` -> `git push origin gh-pages`
6. 記得要到 repo 中將 GitHub Pages 的 source 設定成 `gh-pages branch`
7. 開啟 GiHub Pages 中顯示的網址即可

# 測試與正式發佈的版本

> 推送錯誤的版本到 github 上，會讓 css 的路徑錯誤

- 測試 : `jekyll serve`，會在 `_site/xxx.html` 的 `canonical` 中使用 localhost 的位置
- 正式 : `jekyll build`，在 `_site/xxx.html` 則是用 `_config.yml` 中的 url 設定
- 推送正式版本到 github 上
	1. `cd _site/`
	2. `git push -u origin gh-pages`
- 無論使用 `serve` or `build` 等指令，都會重新在 `_site/` 中，重新建立相關資料，我在猜，當文章過多，速度應該會比較慢。

# Note_1 : 更新 osx ruby

```
\curl -sSL https://get.rvm.io | bash -s stable
rvm list known
rvm install ruby-2.4
rvm use ruby-2.4 --default
ruby -v
```

# Note_2 : 建立草稿與預覽

1. 因為預設不會產生 `_drafts/` 的資料夾，所以需要手動建立
2. 預覽草稿 : 使用 `jekyll serve` or `jekyll build` 時，加入 `--drafts` 參數， ex : `jekyll build --drafts` 
