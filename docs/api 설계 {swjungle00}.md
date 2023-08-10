---
aliases: 
tags: 
description:
title: api 설계 {swjungle00}
created: 2023-08-10T10:50:35
updated: 2023-08-10T11:01:31
---
- GET `/main/<string:user_id>`: html 렌더
- POST `/main/<string:user_id>`: 새 chat 생성
- GET `/api/history/<string:user_id>`: 유저의 히스토리 리스트를 가져옴
- GET `/api/chat/<string:chat_id>`: chat 하나를 가져옴
- POST `/api/chat/<string:chat_id>`: chat regeneration
