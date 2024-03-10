---
aliases: 
tags: 
description:
title: DTO Validation using class-validator {NestJS}
created: 2024-03-10T15:51:47
updated: 2024-03-10T16:02:26
---
- <https://docs.nestjs.com/techniques/validation#auto-validation>
- [[Validation {typeorm}]]
---
`ValidationPipe` 객체를 `app` 의 global pipe로 추가하게 된다면, 컨트롤러가 인자에 들어온 DTO 타입을 해석하고 유효성 검사 및 자동 DTO 인스턴스 생성까지 해준다.

- `app.main.ts`

```ts
async function bootstrap() {
	...
	app.useGlobalPipes(new ValidationPipe());
	...
}
```

- `user.controller.ts`

```ts
@Post()
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
