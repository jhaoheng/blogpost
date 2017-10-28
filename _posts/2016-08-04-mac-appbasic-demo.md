---
layout: post
title: '【mac app】basic demo'
date: 2016-08-04 07:26
comments: true
categories: 
---
# Feature

1. NSStatusBar add menu.
2. When app launched , no any window controller init.(service hidden in back)
3. Don't show app on the dock.

## how

- [demo](https://github.com/jhaoheng/demo-osxapp)

1. Use
	- NSStatusBar *statusBar
  - NSStatusItem *statusItem
  - NSMenu *theMenu
2. storyboard -> select window controller -> attributes inspector -> 取消 is initial controller
3. in plist，set key 'Application is agent (UIElement)' = YES