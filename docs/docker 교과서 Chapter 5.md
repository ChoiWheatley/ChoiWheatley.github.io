---
aliases: 
tags: 
description:
title: docker 교과서 Chapter 5
created: 2024-10-18T20:16:01
updated: 2024-10-18T20:16:02
---
도커 레지스트리는 도커 이미지를 저장하는 보관소로, 별도로 지정하지 않으면 도커허브 (`github.io`)가 기본 설정된다. 따라서 우리가 줄기차게 pull 받았던 `diamol/base` 이미지를 예로 들면 아래와 같은 명칭으로 치환된다:

```
docker.io/diamol/base:latest
^         ^      ^    ^
|         |      |    └── tag
|         |      └─────── image
|         └────────────── group
└──────────────────────── registry domain
```