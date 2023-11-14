---
aliases: 
tags: 
description:
title: oauth2 {nest.js} {passport}
created: 2023-11-13T20:38:32
updated: 2023-11-14T15:15:25
---
- [[0018.1 Nest.js 🪺]]
- [dev.to 블로그글](https://dev.to/tugascript/nestjs-authentication-with-oauth20-configuration-and-operations-41k)
- [[oauth {django}]]
- [google oauth 문서](https://developers.google.com/identity/protocols/oauth2/web-server?hl=ko)
- [@nestjs/passport 문서](https://docs.nestjs.com/recipes/passport#implementing-passport-strategies) passport 패키지 래퍼인 @nestjs/passport에 대해서 알아본다.
___

## overview

strategy 생성하고 모듈과 서비스 생성하고, 직접적으로 strategy 코드를 호출하지는 않고 `AuthModule`이라는 가드를 어노테이션으로 붙여놓는걸로 구현 가능. controller에서 인가가 필요한 엔드포인트에 가드를 걸어놓는다. 실제 authenticate 단계를 위해서 built-in passport guards를 사용함.

- strategy가 하는 일
	- retrieving credentials
	- running verify function
	- creating `user` property

```ts
// src/auth/kakao.strategy.ts
export class KakaoStrategy extends PassportStrategy(Strategy, 'kakao') {
  constructor() {
    super({
      clientID: process.env.KAKAO_CLIENT_ID,
      clientSecret: process.env.KAKAO_CLIENT_SECRET,
      callbackURL: 'http://chltm.mooo.com:3000/auth/kakao',
    });
  }
  async validate(
    accessToken: string,
    refreshToken: string,
    profile: any,
    done: any,
  ) {
    const { id, username, email, provider } = profile;
    console.log(profile);
    const user = {
      id,
      username,
      provider,
      email,
      accessToken,
      refreshToken,
    };
    done(null, user);
  }
}
```

- controller가 하는 일
	- `AuthGard`를 등록하여 사용자 인가작업에 대한 책임을 회피
	- 주입받은 service 메서드를 호출하여 원하는 작업을 대신함.
	- 결국 얘는 책임전가만 열심히 하는 녀석임

```ts
@Controller('auth')
export class AuthController {
  constructor(private readonly authService: AuthService) {}

  @Get('kakao')
  @UseGuards(AuthGuard('kakao'))
  async kakaoAuth(@Req() req): Promise<any> {
    return this.authService.kakaoLogin(req);
  }
}
```

- service가 하는 일
	- `AuthGuard`가 authorization server에 요청을 날려 얻은 유저정보를 `req.user` 에 넣었을 것임.

## Strategy

1. 인증 단계를 JWT, OAuth, username/password strategy로 선택할 수 있음.
2. 검증단계는 verify callback을 사용하게 된다.

## Kakao Oauth2

<https://www.passportjs.org/packages/passport-kakao/>

```
npm i passport-kakao
```

새 애플리케이션을 등록 ⟶ 카카오 로그인 탭에서 활성화 + OpenID Connect + Redirect URI 설정 ⟶ 플랫폼 탭에서 Web 사이트 도메인 추가 ⟶ 앱 키 탭에서 각각 REST API 키와 Admin 키를 Client ID & Secrete Key에 대응하여 접속

email이 필수동의가 안될 것이다. 이 경우, *비즈니스* 탭으로 가서 개인 개발자 비즈 앱을 등록하면 된다.
