---
aliases: 
tags: 
description:
title: oauth2 {nest.js} {passport}
created: 2023-11-13T20:38:32
updated: 2023-11-14T12:39:32
---
- [[0018.1 Nest.js 🪺]]
- [dev.to 블로그글](https://dev.to/tugascript/nestjs-authentication-with-oauth20-configuration-and-operations-41k)
- [[oauth {django}]]
- [google oauth 문서](https://developers.google.com/identity/protocols/oauth2/web-server?hl=ko)
- [@nestjs/passport 문서](https://docs.nestjs.com/recipes/passport#implementing-passport-strategies) passport 패키지 래퍼인 @nestjs/passport에 대해서 알아본다.
___

## TL;DR

## Strategy

1. 인증 단계를 JWT, OAuth, username/password strategy로 선택할 수 있음.
2. 검증단계는 verify callback을 사용하게 된다.
