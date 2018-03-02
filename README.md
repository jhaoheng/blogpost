# build
- localhost : `jekyll server -b ''`, 因此 theme 在 baseurl 的 localhost 與 gh-pages 在 deploy 會產生衝突，故 config 設定的 baseurl 以 deploy 為主
- deploy : `jekyll build`

# deploy
- gh-pages : `cd _site && git add . && git commit -m "update" && git push`

# update

- read more 
    - `_config.yml` : 增加 key-value `excerpt_separator: <!--more-->`
    - `index.html` : 
        - 更新 `{{ post.content }}` 為 `{{ post.excerpt }}`
        - 增加 `<p><a class="btn btn-sm btn-primary" href="{{ post.url }}/#read-more" role="button">Read more <i class="fa fa-arrow-circle-right"></i></a>`
    - `your_blog_new_post.md` : 文章加入 `<!--more-->` 到你想切割的位置

# github & jekyll 相關設定參考
https://jhaoheng.github.io/blogpost/github&jekyll/

# Post need notice

> layout: post
title: "【Docker:mariadb】set init when first runnging"
date: 2017-11-03 12:01
categories: docker mairadb

- 注意 日期 只能是 : `yy-mm-dd hh:mm`

# Theme : HPSTR Jekyll Theme

They say three times the charm, so here is another free responsive Jekyll theme for you. I've learned a ton since open sourcing [my first two themes](https://mademistakes.com/work/jekyll-themes/), and wanted to try a few new things this time around. If you've used my previous themes most of this should be familiar territory.

**Compatible with Jekyll 3.0 and up.**

## What HPSTR brings to the table:

* Modern and minimal design.
* Responsive templates for post, page, and post index `_layouts`. Looks great on mobile, tablet, and desktop devices.
* Gracefully degrades in older browsers. Compatible with Internet Explorer 8+ and all modern browsers.  
* Sweet animated menu with support for drop-downs.
* Optional [Disqus](http://disqus.com) comments and social sharing links.
* [Open Graph](https://developers.facebook.com/docs/opengraph/) and [Twitter Cards](https://dev.twitter.com/docs/cards) support for a better social sharing experience.
* Simple [custom 404 page](http://mmistakes.github.io/hpstr-jekyll-theme/404.html) to get you started.
* [Syntax highlighting](http://mmistakes.github.io/hpstr-jekyll-theme/code-highlighting-post/) stylesheet to make your code examples look snazzy
* [Available in Spanish](https://github.com/cruznick/hpstr-jekyll-theme/tree/es). Thanks [@cruznick](https://github.com/cruznick)!

![HPSTR Theme Preview screenshot](http://mmistakes.github.io/hpstr-jekyll-theme/images/hpstr-jekyll-theme-preview.jpg)

---

## Getting Started

HPSTR takes advantage of SCSS and data files to make customizing easier. This theme requires Jekyll 3.x and will not work with older versions properly.

To learn how to install and use this theme check out the [Setup Guide](https://mmistakes.github.io/hpstr-jekyll-theme/theme-setup/) for more information.
