---
aliases: 
tags: 
description:
title: GDB 디버거 공부하기
created: 2023-09-10T13:01:48
updated: 2023-09-10T14:20:07
---

<iframe title="Quick Intro to gdb" src="https://www.youtube.com/embed/xQ0ONbt-qPs?feature=oembed" height="150" width="200" allowfullscreen="" allow="fullscreen" style="aspect-ratio: 1.33333 / 1; width: 100%; height: 100%;"></iframe>

<iframe title="CppCon 2016: Greg Law “GDB - A Lot More Than You Knew&quot;" src="https://www.youtube.com/embed/-n9Fkq1e6sg?feature=oembed" height="113" width="200" allowfullscreen="" allow="fullscreen" style="aspect-ratio: 1.76991 / 1; width: 100%; height: 100%;"></iframe>

start - begin exexuting your program

list - examine your source code from within the debugger

step - execute the next line of your program  
next - execute the next line of your program, but if it's a subroutine call, treat the entire subroutine as a single line

print - examine the contents of a variable  
x - examine memory directly  
x/b - 1 바이트씩 examine 하겠소  
x/10b - 1바이트씩 10 개를 examine 하겠소.

watch, rwatch - set a watch for when a variable is written or read: return to the debugger once this happens  
break - set a breakpoint: return to the debugger when this line of code is about to be executed  

```
gdb> break /Full/path/to/service.cpp:45
```

info watch - show info on watchpoints  
info break - show info on breakpoints  
delete # - delete watchpoint or breakpoint "#"

cont - continue from a breakpoint, watchpoint, step, next, etc.; basically begin running your program from where it left off

set var name=value   set the value of variable "name" to "value"

bt - show the call frames for your program  
frame # - set the current frame to #. Variables you reference etc. will be those within that context.

quit - leave the debugger

[https://freemmer.tistory.com/31](https://freemmer.tistory.com/31)  
  
[https://stackoverflow.com/questions/14581837/gdb-how-to-print-the-current-line-or-find-the-current-line-number](https://stackoverflow.com/questions/14581837/gdb-how-to-print-the-current-line-or-find-the-current-line-number)  
- tui enable : 터미널 GUI를 활성화. 완전 개꿀이잖아  
[http://cloudrain21.com/gdb-tui-text-user-interface](http://cloudrain21.com/gdb-tui-text-user-interface)  
- focus src : 소스창 이동가능  
- focus cmd : 커맨드 창으로 포커스 이동  
-  
  
- run (r)  
- quit  
- kill (k)  
- break (b) <function / file:function / file:line number / Class::memberFunction >  
- info <b / locals / variables / registers / frame / args / catch / signals / set / functions >  
  
- step (s)  
- next (n)  
- continue (c)  
- until (u) // for 문에서 빠져나와 다음 브레이크까지  
  
- finish // 함수 실행하고 빠져나온다  
- return / 함수 마저 실행하지 않고 빠져나온다  
  
- watch bCheck // bCheck 값이 변경될 때마다 브레이크가 걸리도록  
- rwatch bCheck // bCheck 값이 읽혀질 때마다 브레이크가 걸리도록  
- awatch bCheck // bCheck 값이 읽기, 쓰기 경우 모두 브레이크  
  
- print (p) <statement / func / structure / ... >  
  
- list (l) // 소스코드 보기  
  
- display pVal // pVal을 매번 화면에 표시  
- undisplay 1 // 1번 디스플레이 설정을 지운다.  
- enable display 1 // 1번 디스플레이를 활성화한다  
- disable diplay 1 // 1번 디스플레이를 비활성화한다  
- backtrace (bt) // 프로그램 스택을 보여준다.  
  
[https://stackoverflow.com/questions/38803783/how-to-automatically-refresh-gdb-in-tui-mode](https://stackoverflow.com/questions/38803783/how-to-automatically-refresh-gdb-in-tui-mode)  
- refresh : tui 창 새로고침
