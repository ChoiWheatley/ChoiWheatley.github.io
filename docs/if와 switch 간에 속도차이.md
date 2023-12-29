---
aliases: 
tags: 
description:
title: if와 switch 간에 속도차이
created: 2023-09-07T21:16:25
updated: 2023-09-07T21:20:56
---
나는 무슨 언어던 간에 복잡한 depth를 가진 if문을 사용하는 것을 꺼려한다. 가능한 많은 인자를 먼저 넣어놓고 케이스로 묶어서 다루는 switch 또는 python의 match 구문을 선호한다. 그런데 if문과 switch 사이에 CPU 사이클의 차이가 존재한다는 말을 듣고 조사해볼 필요가 생겼다.

> In most languages, `switch` only accepts primitive types as key and constants as cases. This means it can be optimized by the compiler using a jump table which is very fast. [SO answer](https://stackoverflow.com/a/680664/21369350)  
> 대부분의 언어에서 `switch` 문은 primitive 타입만 허용하고 case로는 상수만을 허용합니다. 그 말은 즉슨 컴파일러가 일종의 점프 테이블을 생성하여 최적화 하는것이 가능하다는 것을 뜻하며, 속도가 매우 빨라집니다.
