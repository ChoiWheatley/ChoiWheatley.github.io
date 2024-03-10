---
aliases: 
tags: 
description:
title: 2024-03-10 wishfunding
created: 2024-03-10T13:34:36
updated: 2024-03-10T15:50:08
---

## timestamp 칼럼을 어떻게 표현하더라?

- <https://jojoldu.tistory.com/600>

내장 `Date` 객체를 사용하기엔 불변타입이 아니라서 에러를 자주 일으킬 가능성이 높다고 한다. 그래서 [js-joda](https://www.npmjs.com/package/js-joda)를 제안하고 있는 블로그이다.

- [[DateColumn Decorators {typeorm}]] 를 사용하면 row 생성, 수정, 삭제에 대한 시간을 자동으로 관리할 수 있어 편리하다.

> 하지만 나는 임의의 Date를 저장해야 할 필요가 있는데요?

`endAt` 이라는 칼럼은 펀딩이 끝나는 시간을 저장한다. 그러므로 `DateColumn` 데코레이터만으로는 처리할 수 없게된다.

```ts
   /**
	 * https://typeorm.io/entities#column-types
	 *
     * TODO - timestamptz를 사용할지, 일반 date를 사용할지 결정해야함.
     * timestamptz는 시간 & 타임존을 포함하고 date는 말 그대로 날짜만 저장함
     */
    @Column("date")
    // @Column("timestamptz")
    endAt: Date;
```

## ✌️ Typeorm Column Validation

save를 할 때 금액칸에 음수가 들어갈 경우 Typeorm에서 잘못된 값이 들어갔는지 여부를 판단하여 에러를 발생하게 만들어 줄 수 있다면 좋을 것 같다.

[[Validation {typeorm}]]

## 칼럼 이름을 내가 직접 정의할 수는 없을까?

`OneToMany`, `ManyToOne`으로 프로퍼티들을 만들다보면 ER 다이어그램을 작성하며 열심히 정의했던 FK들이랑 이름이 달라지는 경우가 발생하게 되었다. 그래서 ORM이 자동으로 FK를 만들어 줄 때 그 이름을 내가 먹일 수는 없는걸까?

## Uuid가 필요한 테이블

- `Funding`

혹시 더 있나? `Funding` 테이블에 `fundUuid` 프로퍼티 추가해야함.

## DTO와 Typeorm

Data Transfer Object로, 데이터를 전달하기 위해 있는 객체임. DTO 객체의 클래스를 정의하면 컨트롤러에서 타입을 자동으로 DTO로 변환해서 받아볼 수 있다. recre-backend에서의 예시를 다시 복습해보자:

- `user.controller.ts`

```ts
export class UserController {
	...
    @Post()
    create(@Body() createUserDto: CreateUserDto) {
        return this.userService.createUser(createUserDto);
    }
}
```

- `user.service.ts`

```ts
export class UserService {
	...
    createUser(createUserDto: CreateUserDto): Promise<User> {
        const user: User = new User();
        user.nickname = createUserDto.nickname;
        user.profileImage = createUserDto.profileImage;
        user.provider = createUserDto.provider;
        user.email = createUserDto.email;
        user.createdDt = new Date(Date.now());
        return this.userRepository.save(user);
    }
}
```

DTO에 요청시 프로퍼티들의 타입에 제한을 걸 수 있고, -1원과 같이 말이 안되는 경우도 [[Validation {typeorm}]]을 통해 걸러낼 수 있다고 한다.

[[DTO Validation using class-validator {NestJS}]]

## 회의 안건..

- [ ] 현진이 github 계정 ChoiWheatley / wishlist_funding_dbdiagram 리포지토리에 collaborator 권한 부여
