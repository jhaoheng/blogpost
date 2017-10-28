---
layout: post
title: '【xcode】create framework'
date: 2016-06-13 06:18
comments: true
categories: [xcode ios]
---
## Why

直接在 xcode 中，產生出適合 `armv7 -arch armv7s` 與 `x86_64` 的 framework
若透過預設的方法產出，會有兩種版本，必須再使用 `lipo create` 進行合併
此方法是透過 script 在 compile 時，直接進行 build 與 `lipo create` 的動作

### 基本設定
1. 設置靜態庫，將 Mach-o type 從動態設定為 Static Library
```
|-MyFirstFramework
	|-TARGETS
  	|-Build Settings
    	|-Linking
      	|-Mach-o Type
```

## How

1. open xcode and select [Framework & Library] -> [Cocoa Touch Framework]

2. Build Phases
	- Target Dependencies
	- [Compile Sources](http://stackoverflow.com/questions/17198185/what-is-the-purpose-of-compile-sources-in-xcode)
	- [Link Binary With Library](https://developer.apple.com/library/ios/recipes/xcode_help-project_editor/Articles/AddingaLibrarytoaTarget.html)
		- 連接使用其他 framework
	- Headers
		- 讓 framework 中的檔案開放程度
	- Copy Bundle Resources
		- 要使用的 bundle 物件檔

3. 產生 framework x2（真機運行與模擬機運行）
	- 要注意的是 xxx-name.framework，只是一包資料夾，要進行合併的是底下的 [xxx-name] 
  - 真機路徑(path_1) : Debug-iphoneos/[name].framework/[name]
  - 模擬機路徑(path_2) : Debug-iphonesimulator/[name].framework/[name]
  - `lipo -create path_1 path_2 -output [name]`
  - 產生出的 name 為執行檔，copy `Debug-iphoneos/[name].framework` 到適當位置
  - 將剛剛產生出的 [name]，取代掉上一行 framwork 中原本的 [name]
  - `lipo -info [name]`
  - 將整包調整過後的 framework 放入 xcode 測試