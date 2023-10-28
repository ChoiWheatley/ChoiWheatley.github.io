---
aliases: 
tags: 
description:
title: 정글 대 토론회 {swjungle} {pintos}
created: 2023-10-12T18:35:43
updated: 2023-10-13T14:15:57
---
- [[week07-10 {swjungle} {pintos}]]
___

## README

본 파일은 2023-10-12T20:00:00에 진행되는 정글 대 토론회 (by 병철) 발표준비 + 다른 발표자들과의 대화 내용을 기록하기 위해 제작되었습니다. 저는 특별히 [[list {C}#Generic Linked List]] 발표를 준비하려고 하고 같은 리포지토리를 활용하여 발표를 진행하는 세준이는 `child_info`에 대해서 발표한다고 합니다.

## [[list {C}#Generic Linked List|Generic List]]

## child_info

`process.c` 참조

zombie process가 영원히 wait하지 않는 부모를 기다리느라 메모리를 낭비하는 일을 막기 위해 고안한 장부.

## page map level 4 @신병철

- [?] 왜 32비트 운영체제는 테이블 엔트리가 1024개인데 64비트 운영체제에서는 테이블 엔트리가 512개로 줄었나요? 9 * 4 + 12 = 48bit임. 10 * 4 + 12 = 52bit임.
	- 32비트 va는 10비트 엔트리를 사용하여 1024개의 엔트리, 64비트 va는 9비트 엔트리를 사용하여 512개의 엔트리.
	- 4B * 1024 == 8B * 512 == 4096B == 4KB
