---
description:
aliases: 
tags: 
created: 2023-04-12T22:21:27
updated: 2024-01-11T17:06:11
title: owner가 아니라면 raw pointer를 참조해도 괜찮다 - when to use smart pointers
---
- <https://stackoverflow.com/questions/64443556/modern-c-approach-for-providing-optional-arguments>
- [Smart Pointers - What, Why, Which?](https://ootips.org/yonat/4dev/smart-pointers.html)
- <https://stackoverflow.com/questions/36673391/c-linked-list-using-smart-pointers#36675943>
---

> Never use owning raw pointers and delete, **except in rare cases when implementing your own low-level data structure** (and even then keep that well encapsulated inside a class boundary).

리스트나 벡터와 같이 low-level 구조에서는 raw pointer가 더 낫다고 한다.
