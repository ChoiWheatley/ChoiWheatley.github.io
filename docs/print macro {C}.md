---
aliases: 
tags: 
description:
title: print macro {C}
created: 2023-09-20T14:41:24
updated: 2023-09-20T15:06:04
---
디버깅 할 때 `x: 1` 이런 식으로 변수명도 같이 나와줬으면 하는 바램을 매크로로 충족시켜주었다. 얼마든지 다른 타입 (`%x`, `%h` 등등)으로도 확장이 가능하다.

```c
#define PRINT_D(x) printf("%s: %d\n", (#x), (int)(x))
#define PRINT_L(x) printf("%s: %ld\n", (#x), (long)(x))
#define PRINT_S(x) printf("%s: \"%s"\n", (#x), (char *)(x))
```
