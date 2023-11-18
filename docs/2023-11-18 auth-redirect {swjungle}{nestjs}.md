---
aliases: 
tags: 
description:
title: 2023-11-18 auth-redirect {swjungle}{nestjs}
created: 2023-11-18T22:32:08
updated: 2023-11-18T22:42:21
---
- [[week14-18 {swjungle}{my own weapon}{nestjs, socketio}]]
- [[2023-11-18 CORS 정면추돌]]
- [[HTTP header CORS policy - 'Access-Control-Allow-Origin']]
___

## 목표

1. 기존에 OAUTH2 redirection url을 직접 리디렉션 하는 코드를 axios(in client) + response.json(in server)를 활용하도록 바꾼다.
2. NextJS 서버와 NestJS 서버를 독립적인 공간에서 돌렸을때 CORS 에러가 발생하도록 만든다.
3. 크로스 오리진 헤더를 설정하여 본 문제를 해결한다.
