---
layout: post
title: '【xcode】Configurations/ Preprocessor Macros/ scheme'
date: 2016-08-31 06:26
comments: true
categories: Xcode
---
# why

In dev / release env, switch the default global variable : 
- url
- other env variable

# xcode setting

1. PROJECT -> info -> Configurations，default only have "Debug" & "Release" tag
  - Use scheme to switch the tag
2. goto Targets，choose the "Macros" from target
	- Build Setting -> Preprocessor Macros, you can find "Debug" & "Release" tag
	- In Macros, set your tag & value

# scheme switch

Debug & Realese tags are set by xcode Configuration

1. edit scheme.
2. select Run tag, switch the "Build Configuration" tag to change your build setting.

# coding example

you can create a config.h or .pch to set your info. 

```
#if DEBUG
#define a symbol
#elseif RELEASE
#define a symbol
#else
#define a symbol
#endif
```

```
#if DEBUG==1
#define debug_only YES
#else
#define debug_only NO
#endif
```

Notice `#if` and `#ifdef` is diff.