---
aliases: 
tags: 
description:
title: padding bytes in struct {C}
created: 2023-09-02T00:05:07
updated: 2023-09-02T00:08:15
---

char가 9개, int가 1개인 구조체가 있다고 하자. 전체 점유하는 메모리의 크기는 9 + 4 = 13이다. 이 구조체의 인스턴스 크기는 몇바이트일까?

```C
struct charchar {
  char a;
  char b;
  char c;
  char d;
  char e;
  char f;
  char g;
  char h;
  char i;
  int dd;
}c;
printf("%d\n", sizeof(c));
```

결과값은 16이다. int의 크기인 4바이트의 배수가 기준이 되어 13과 가장 가까운 4의 배수인 16이 된다.
