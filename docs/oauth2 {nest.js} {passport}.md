---
aliases: 
tags: 
description:
title: oauth2 {nest.js} {passport}
created: 2023-11-13T20:38:32
updated: 2023-11-14T12:54:43
---
- [[0018.1 Nest.js 🪺]]
- [dev.to 블로그글](https://dev.to/tugascript/nestjs-authentication-with-oauth20-configuration-and-operations-41k)
- [[oauth {django}]]
- [google oauth 문서](https://developers.google.com/identity/protocols/oauth2/web-server?hl=ko)
- [@nestjs/passport 문서](https://docs.nestjs.com/recipes/passport#implementing-passport-strategies) passport 패키지 래퍼인 @nestjs/passport에 대해서 알아본다.
___

## overview

strategy 생성하고 모듈과 서비스 생성하고, 직접적으로 strategy 코드를 호출하지는 않고 `AuthModule`이라는 가드를 어노테이션으로 붙여놓는걸로 구현 가능. controller에서 인가가 필요한 엔드포인트에 가드를 걸어놓는다. 실제 authenticate 단계를 위해서 built-in passport guards를 사용함.

- guards가 하는 일
	- retrieving credentials
	- running verify function
	- creating `user` property


## Strategy

1. 인증 단계를 JWT, OAuth, username/password strategy로 선택할 수 있음.
2. 검증단계는 verify callback을 사용하게 된다.
