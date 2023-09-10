---
aliases: 
tags: 
description:
title: GDB 디버거 공부하기
created: 2023-09-10T13:01:48
updated: 2023-09-10T13:02:55
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
info watch - show info on watchpoints  
info break - show info on breakpoints  
delete # - delete watchpoint or breakpoint "#"

cont - continue from a breakpoint, watchpoint, step, next, etc.; basically begin running your program from where it left off

set var name=value   set the value of variable "name" to "value"

bt - show the call frames for your program  
frame # - set the current frame to #. Variables you reference etc. will be those within that context.

quit - leave the debugger
