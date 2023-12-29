---
aliases: 
tags: 
description:
title: redirect URI mismatch 문제 {OAUTH2} {nest.js}
created: 2023-11-14T21:21:34
updated: 2023-11-15T10:15:19
---
- [[oauth2 {nest.js} {passport}]]
-  ![[스크린샷 2023-11-14 오후 9.21.49.png]]

___

## 두 가지 엔드포인트가 있다.

- `/auth/google` 
	- 유저가 third party login을 시작할 때 요청하는 엔드포인트. `UseGuards(AuthGuard('google'))`에서 걸려 구글 로그인 페이지로 리디렉션이 된다.
- `/auth/google/redirect`
	- 구글 로그인을 마쳤을 때 구글측에서 자동으로 리디렉션 응답을 보내 호출될 엔드포인트

![[Drawing 2023-11-14 22.36.56.excalidraw]]  
![[Pasted image 20231114232402.png]]

## 구현 플로우

- `PassportStrategy`를 상속한 google/kakao/naver strategy의 역할은 컨트롤러에게 컨텍스트가 넘어가기 전에 유저 데이터를 넘긴다. (여러가지 더 있긴 함) [implementing passport strategies](https://docs.nestjs.com/recipes/passport#implementing-passport-strategies)
- `auth.service.ts` 파일은 유저 데이터가 담긴 req의 email를 기반으로 DB에 쿼리를 날린다. 만약 유저가 존재한다면 그대로 리턴하고, 없다면 회원가입을 시켜 리턴한다.
- `auth.controller.ts`는 각 strategy 별로 두개씩 엔드포인트를 정의한다. 예를 들어 구글 strategy라면, `/auth/google`, `/auth/google/redirect`인 식이다. 이렇게 만드는 이유는 OAUTH2.0 기반 인증방식의 특징과 연관이 있다. 아래 이미지를 본다면 처음에 유저가 본 서버에 요청할 때에는 전자를, 구글 로그인을 마친 뒤에 다시 본 서버에 authorization code를 추가한 채로 요청할 때에는 후자를 사용한다.

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
