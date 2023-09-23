---
aliases: 
tags: 
description:
title: GDB 디버거 공부하기
created: 2023-09-10T13:01:48
updated: 2023-09-23T23:33:04
---
- 참조 사이트들
- [https://freemmer.tistory.com/31](https://freemmer.tistory.com/31)  
- <iframe title="Quick Intro to gdb" src="https://www.youtube.com/embed/xQ0ONbt-qPs?feature=oembed" height="150" width="200" allowfullscreen="" allow="fullscreen" style="aspect-ratio: 1.33333 / 1; width: 50%; height: 50%;"></iframe>
- <iframe title="CppCon 2016: Greg Law “GDB - A Lot More Than You Knew&quot;" src="https://www.youtube.com/embed/-n9Fkq1e6sg?feature=oembed" height="113" width="200" allowfullscreen="" allow="fullscreen" style="aspect-ratio: 1.76991 / 1; width: 50%; height: 50%;"></iframe>
---  
- `run (r)  [additional command line args]`
- quit  
- kill (k)  
- break (b) <function / file:function / file:line number / Class::memberFunction >  

	```
	gdb> break /Full/path/to/service.cpp:45
	```

- `info <b / locals / variables / registers / frame / args / catch / signals / set / functions >`
- start - begin exexuting your program
---
- step (s)  execute the next line of your program  
- next (n)  execute the next line of your program, but if it's a subroutine call, treat the entire subroutine as a single line
- continue (c)  continue from a breakpoint, watchpoint, step, next, etc.; basically begin running your program from where it left off
- until (u) // for 문에서 빠져나와 다음 브레이크까지  
- finish // 함수 실행하고 빠져나온다  
- return / 함수 마저 실행하지 않고 빠져나온다  
---  

## watch and display

<https://www.cse.unsw.edu.au/~learn/debugging/modules/gdb_watch_display/>

- watch bCheck // bCheck 값이 변경될 때마다 브레이크가 걸리도록  
- rwatch bCheck // bCheck 값이 읽혀질 때마다 브레이크가 걸리도록  
- awatch bCheck // bCheck 값이 읽기, 쓰기 경우 모두 브레이크  
- watch, rwatch - set a watch for when a variable is written or read: return to the debugger once this happens  
- delete # - delete watchpoint or breakpoint "#"
- print (p) <statement / func / structure / ... >  
	- `p $eax` - 마지막 함수가 호출한 값 출력
- list (l) // 소스코드 보기  
- display pVal // pVal을 매번 화면에 표시  
- undisplay 1 // 1번 디스플레이 설정을 지운다.  
- enable display 1 // 1번 디스플레이를 활성화한다  
- disable diplay 1 // 1번 디스플레이를 비활성화한다  
- backtrace (bt) // 프로그램 스택을 보여준다.  
--- 

## MISC

- `frame #` - set the current frame to #. Variables you reference etc. will be those within that context.
- `x` - examine memory directly  -- 주로 배열, 문자열 검사할 때 유용함.
	- <https://visualgdb.com/gdbreference/commands/x>
	- `x/b` - 1 바이트씩 examine 하겠소  
	- `x/10b` - 1바이트씩 10 개를 examine 하겠소.
- set var name=value   set the value of variable "name" to "value"
- `directory` - 현 디렉토리에 있는 파일들을 새로고침
---

## TUI

- [https://stackoverflow.com/questions/38803783/how-to-automatically-refresh-gdb-in-tui-mode](https://stackoverflow.com/questions/38803783/how-to-automatically-refresh-gdb-in-tui-mode)  
- [https://stackoverflow.com/questions/14581837/gdb-how-to-print-the-current-line-or-find-the-current-line-number](https://stackoverflow.com/questions/14581837/gdb-how-to-print-the-current-line-or-find-the-current-line-number)  
- [http://cloudrain21.com/gdb-tui-text-user-interface](http://cloudrain21.com/gdb-tui-text-user-interface)  
- tui enable : 터미널 GUI를 활성화. 완전 개꿀이잖아  
- focus src : 소스창 이동가능  
- focus cmd : 커맨드 창으로 포커스 이동  
- refresh : tui 창 새로고침

## gdb-dashboard

<https://github.com/cyrus-and/gdb-dashboard>
