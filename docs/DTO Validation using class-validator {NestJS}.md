---
aliases: 
tags: 
description:
title: DTO Validation using class-validator {NestJS}
created: 2024-03-10T15:51:47
updated: 2024-03-10T17:33:11
---
- <https://docs.nestjs.com/techniques/validation>
- [[Validation {typeorm}]]
- [DTO를 Entity로 변환하기 # JSON 역직렬화하기](https://velog.io/@dinb1242/DTO-%EB%A5%BC-Entity-%EB%A1%9C-%EB%B3%80%ED%99%98%ED%95%98%EA%B8%B0-DTO-to-Entity-feat.-class-validator) 이 블로그는 더 나아가 DTO ⟶ Entity 변환하는 방법에까지 설명하고 있다.
---
`ValidationPipe` 객체를 `app` 의 global pipe로 추가하게 된다면, 컨트롤러가 인자에 들어온 DTO 타입을 해석하고 유효성 검사 및 자동 DTO 인스턴스 생성까지 해준다. 

단지 컨트롤러 인자에 작성하는 Dto는 단순히 JS Plain Object의 형태로 들어온다. 따라서 `ValidationPipe`를 사용하여 페이로드를 자동으로 해당 타입에 맞는 DTO 인스턴스로 변환하는 과정이 필요하다. 

- `app.main.ts`

```ts
async function bootstrap() {
	...
	app.useGlobalPipes(new ValidationPipe({
		transform: true,  // 자동으로 JS Object를 DTO로 변환 (Globally)
	}));
	...
}
```

- `user.controller.ts`

```ts
@Post()
@UsePipes(new ValidationPipe( { transform: true } )) // 자동으로 JS Object를 DTO로 변환 (method-level)
create(@Body() createUserDto: CreateUserDto) {
	return 'This action adds a new user';
}
```

- `user.dto.ts`

NestJS 또한 [class-validator](https://github.com/typestack/class-validator#validation-decorators)를 사용하는구나.

```ts
export class CreateUserDto {
	@IsEmail()
	email: string;

	@IsNotEmpty()
	password: string;
}
```

Body만 검사할 줄 아냐? Param도 가능하다~

```ts
/** find-one-params.ts */
export class FindOneParams {
	@isNumberString()
	id: number;
}

@Get(':id')
findOne(@Param() params: FindOneParams) {
	...
}
```
