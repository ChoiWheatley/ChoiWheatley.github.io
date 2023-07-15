---
description:
aliases: 
tags: 
created: 2023-04-12T22:51:15
updated: 2023-07-15T21:33:05
title: extern 키워드 + 컴파일 인자로 여러개의 cpp 파일을 동일한 스코프에  때려넣자
---
extern 키워드로 여러개의 cpp 파일을 컴파일 하고자 할 땐

```
g++ a.cpp b.cpp
```

이런 식으로 인자에 다 때려박으면 된다.  
https://stackoverflow.com/questions/40918105/c-extern-and-gcc
