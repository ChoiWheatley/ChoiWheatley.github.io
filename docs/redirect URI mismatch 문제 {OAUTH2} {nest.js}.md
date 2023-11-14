---
aliases: 
tags: 
description:
title: redirect URI mismatch 문제 {OAUTH2} {nest.js}
created: 2023-11-14T21:21:34
updated: 2023-11-14T21:28:37
---
- [[oauth2 {nest.js} {passport}]]
-  ![[스크린샷 2023-11-14 오후 9.21.49.png]]

___

## 두 가지 엔드포인트가 있다.

- `/auth/google` 
	- 유저가 third party login을 시작할 때 요청하는 엔드포인트. `UseGuards(AuthGuard('google'))`에서 걸려 구글 로그인 페이지로 리디렉션이 된다.
- `/auth/google/redirect`
	- 구글 로그인을 마쳤을 때 구글측에서 자동으로 리디렉션 응답을 보내 호출될 엔드포인트



## TroubleShooting

엔드포인트 정의부 마지막에는 꼭 `/`를 붙여주세요.

**before**

```ts
@Controller('auth')
```

**after**

```ts
@Controller('auth/')
```
