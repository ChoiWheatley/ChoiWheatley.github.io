---
links:
status:
aliases: 
tags:
  - scrap
description: 
title: tasks, microtasks, queues and schedules {js} {TODO}
created: 2023-11-10T20:56:30
updated: 2024-12-31T23:19:43
---
- <https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/>
- [[0018 Javascript ☕️]]
___

## README

js의 Promise객체에 대해서 이해를 하는데 많은 어려움을 겪고있다. 왜 Busy waiting은 JS에서 무의미한건지, 오로지 기다리는 작업만 동시에 수행할 수 있는지에 대한 의문점을 남겨놓고 이벤트 루프에 관한 외국형님의 블로그 글을 접하게 됐다.

이 블로그 글은 JS를 탁월하게 잘하는 분이 다양한 큐들에 대한 상태변화를 시각적인 모습으로 표현하고 있기 때문에 한 번 각잡고 본다면 유익할 것 같다.

## 여러가지 종류의 큐

- Task
- Microtask
- JS Stack
