---
layout: post
title: '【erlang】Hello Erlang!'
date: 2016-11-10 22:59
comments: true
categories: 
---
# hello_erlang_1

- hello.erl
	- code  
```
-module(hello).
-export([print/0]).
print() -> 
io:format("World!~n").
```

  - 解析
    - `-module`
    - `-export`
    - `[print/0]` : 函數名稱 print , 參數 0 個
    - `io:format` : 格式化標準輸出函數，參考 [io : Available control sequences:](http://erlang.org/doc/man/io.html#fread-2)
      - `~n` : Writes a new line.

- test
	- 流程
		1. `erl`
		2. `pwd().`
		3. `ls().`
		4. `c(hello).` : hello 為 hello.erl 的檔案名稱, c() 為編譯此檔案 -> 會產生 hello.beam 的執行擋
		5. `hello:print().`
	- 基本
		- `.` : 每一句皆以此，作為一個斷句

# hello_erlang_2

- hello2.erl
```
-module(hello2).
-compile(export_all).
square(N) -> N*N.
sum([]) -> 0;
sum([Num|Ns]) -> Num + sum(Ns).
sum_square([]) -> 0;
sum_square([Num|Ns]) -> square(Num) + sum_square(Ns).
```

- test
	- `hello2:square(2).` : 4
	- `hello2:sum([1,2,3,4,5]).` : 15
	- `;` : 代表 or
	- `,` : 代表 and

# hello_erlang_3

- hello3.erl
```
solve(1) -> [1];
solve(N) when N rem 2 == 0 -> [N|solve(N div 2)];
solve(N) -> [N|solve(N*3+1)].
set(N) 	-> {ok, [H|T] = solve(N)}.
```

# hello_erlang_4 : regular expressions

- hello4.erl
```
-module(do).
-compile(export_all).

multire([],_) ->
    nomatch;
multire([RE|RegExps],String) ->
    case re:run(String,RE,[{capture,none}]) of
    match ->
        RE;
    nomatch ->
        multire(RegExps,String)
    end.

% 開頭為 Hello, 結尾有 world, 均符合
test(Foo) ->
    test2(multire(["^Hello","world$","^....$"],Foo),Foo).

test2("^Hello",Foo) ->
    io:format("~p matched the hello pattern~n",[Foo]);
test2("world$",Foo) ->
    io:format("~p matched the world pattern~n",[Foo]);
test2("^....$",Foo) ->
    io:format("~p matched the four chars pattern~n",[Foo]);
test2(nomatch,Foo) ->
    io:format("~p failed to match~n",[Foo]).


-define (Email_pattern, "\\b[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\\.[a-zA-Z0-9-]+)*\\b").
-define (Mac_address_pattern, "^([0-9A-Fa-f]{2}[:-]){5}([0-9A-Fa-f]{2})$").

checkPattern(Foo) -> 
  checkPattern2(multire([[?Email_pattern], [?Mac_address_pattern]],Foo),Foo).

checkPattern2([?Email_pattern], Foo) ->
  io:format("email format : ~p ~n", [Foo]);
checkPattern2([?Mac_address_pattern], Foo) ->
  io:format("device format : ~p ~n", [Foo]);
checkPattern2(nomatch, Foo) ->
  io:format("nomatch : ~p ~n", [Foo]). 	
```