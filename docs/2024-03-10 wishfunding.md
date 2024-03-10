---
aliases: 
tags: 
description:
title: 2024-03-10 wishfunding
created: 2024-03-10T13:34:36
updated: 2024-03-10T14:24:04
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
