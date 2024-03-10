---
aliases: 
tags: 
description:
title: Nest .env 파일 관리법 (.dev.env, .local.env 등등)
created: 2024-03-10T00:14:33
updated: 2024-03-10T00:58:30
---
- [docs.nestjs.com / configuration](https://docs.nestjs.com/techniques/configuration#service)
- <https://velog.io/@kimjiwonpg98/NEST-.env-%ED%8C%8C%EC%9D%BC-%EA%B4%80%EB%A6%AC%EB%B2%95-feat.-nestjsconfig> 를 참고했습니다.
---

## Walkthrough

### 설치

```
npm i --save @nestjs/config
```

`ConfigModule`을 가져올 수 있고, `AppModule`에서 이를 세팅함으로써 환경변수를 가져올 수 있다. 기본값은 `.env` 파일을 읽고 `process.env.{{name}}`의 형태로 환경변수를 읽을 수 있다.

### app.module

```ts
import { ConfigModule } from '@nestjs/config';

@Module({
	imports: [
		ConfigModule.forRoot({
			isGlobal: true,
			envFilePath: ['.env'],
			cache: true,
			expandVariables: true,
		})
	]
})
```

[[cross-env {NodeJS}]] 를 사용하면 여러 .env 파일들을 두고 서로 다른 config를 가지고 NestJS를 실행시킬 수 있다고 한다.

## 더 알아볼 것들

- [`isGlobal`과 global module](https://docs.nestjs.com/techniques/configuration#use-module-globally)
- [`ConfigService`를 사용하여 모듈별 별도의 환경변수를 지원하는 방법](https://docs.nestjs.com/techniques/configuration#using-the-configservice)
- [[dotenv NodeJS (TODO)]]가 어떻게 `.env` 파일을 읽을까?
- [yaml, toml, json 파일을 읽어 configuration하기](https://docs.nestjs.com/techniques/configuration#custom-configuration-files)
- [`Joi`를 사용하여 .env 파일을 validate하라](https://docs.nestjs.com/techniques/configuration#schema-validation)
	- 타입 제한 가능
	- 필수로 존재해야 하는 환경변수 강제 가능
- [[dot env 에서 변수를 사용할 수 있다고? {NodeJS} {dotenv-expand}]]
