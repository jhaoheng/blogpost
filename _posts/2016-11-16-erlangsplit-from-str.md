---
layout: post
title: '【erlang】split from str'
date: 2016-11-16 08:11
comments: true
categories: 
---
```
-module(split).

-compile(export_all).

foo() -> 
	Topic = "/a/b/c/d/e/f/g/h",
	Test = splitTopic(Topic),
	io:format("===>final company is : ~p~n",[Test]).

splitTopic(Topic) -> 
    Keys = string:tokens(Topic, "/"),
    [Company, Dtype, Etype, Sid | Others] = Keys,
    io:format("~n~n===Split Topic===~n"),
    io:format("- Company = ~p~n- DeviceType = ~p~n- Event_type = ~p~n- Sid = ~p~n- Others = ~p~n~n", [Company, Dtype, Etype, Sid, Others]),
    rsplit(Company).

rsplit(Company) -> 
    Company.
```