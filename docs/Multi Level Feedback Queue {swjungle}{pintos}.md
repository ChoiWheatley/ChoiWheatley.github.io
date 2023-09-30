---
aliases: 
tags: 
description:
title: Multi Level Feedback Queue {swjungle}{pintos}
created: 2023-09-30T15:05:14
updated: 2023-09-30T15:32:14
---
- [[week07-09 {swjungle} {pintos}]]
___

## README

[[week07-09 {swjungle} {pintos}]] Project 1의 마지막 과제! NICE가 무엇인지, aging 기법을 활용했을 때 어떻게 starvation을 막을 수 있는지 여부를 알아낸 뒤 내가 어떤 것을 구현하여야 하는건지 목표를 파악한 뒤에 빠르게 구현까지 끝내보자.

## Summary

> 4.4 BSD Scheduler와 같은 multi-level-feedback-queue 스케줄러를 구현하여 평균 응답시간을 줄여라.

pintos 명령줄에 `-mlfqs` 옵션을 넣으면 `threads/thread.h`에 선언된 `thread_mlfqs` 전역변수가 TRUE로 수정된다. 이 경우 thread_create를 할 때 지정한 인자 중 우선순위 값을 무시한다.

mlfqs는 우선순위 기부를 하지 않기 때문에 우선순위 기부를 제외한 코드를 따로 호출하도록 만들어야 한다. 