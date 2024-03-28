---
aliases: 
tags: 
description:
title: typeorm 연관 컬럼을 객체가 아니라 id number로 불러오기
created: 2024-03-28T21:44:53
updated: 2024-03-28T21:50:10
---
- [stackoverflow / Only Ids instead of whole instances](https://stackoverflow.com/questions/59831159/typeorm-relationship-only-ids-instead-of-whole-instances)
---

## Question

Typeorm은 연관 컬럼들의 타입이 무조건 엔티티이기 때문에 id를 얻어오기 위해선 무조건 JOIN을 수행해야 한다는 문제점이 발견되었습니다. 

```typescript
import {Entity, PrimaryGeneratedColumn, Column, OneToOne, JoinColumn} from "typeorm";
import {Profile} from "./Profile";

@Entity()
export class User {

    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    name: string;

    @OneToOne(type => Profile)
    @JoinColumn()
    profile: Profile;

}
```

여기에서 User와 연관된 테이블인 Profile에 접근하게 되면 JOIN이 일어납니다. 객체 관계가 아닌, 단순 `profileId: number`로 접근하고 싶다면 어떻게 해야하나요?

## Answer

단순 FK 필드 `profileId`를 프로퍼티에 추가하시면 됩니다.

```typescript
@Entity()
export class User {

    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    name: string;

    @OneToOne(type => Profile)
    @JoinColumn()
    profile: Profile;

    @Column()
    profileId: number;
}
```
