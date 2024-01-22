---
aliases: 
tags:
  - scrap
description: 
title: glibc Malloc Internals
created: 2024-01-20T19:01:37
updated: 2024-01-20T19:02:43
---
- [glibc wiki](https://sourceware.org/glibc/wiki/MallocInternals)
---

리눅스 malloc이 어떻게 동작하는지 글을 읽어보고 arena가 뭐고 chunk가 뭔지, Thread Local Cache (tcache)와 알고리즘에 대해 살펴보자.
