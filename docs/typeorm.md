---
aliases: 
tags: 
description:
title: typeorm
created: 2023-11-27T16:17:25
updated: 2024-04-14T23:03:06
---
- [[0018 Javascript ☕️]]
- [공식문서](https://typeorm.io/)
- [[sequelize, a MySQL ORM for javascript]]
___

## README

sequalize와의 차이점 위주로 CRUD, 엔티티, repository, relation 위주로 **빠르게** 정리하겠습니다.

눈여겨 볼만한 주제들

- entities & columns
- entity manager?
- repository
- associations (relations)
- eager & lazy relations
- cascades

## 톺아보기...

> And your domain logic looks like this:

```ts
const userRepository = MyDataSource.getRepository(User);

const user = new User();
user.firstName = "Timber";
user.lastName = "Saw";
user.age = 25;
await userRepository.save(user);

const allUsers = await userRepository.find();
const firstUser = await userRepository.findOneBy({
	id: 1,
});
const timber = await userRepository.fineOneBy({
	firstName: "Timber",
	lastName: "Saw",
});
timber.age = 26;
await userRepository.save(timber);
const [users, userCount] = await userRepository.findAndCount();
await userRepository.remove(timber);
```

- [[Column Types {typeorm}]]
- [[DataSource {typeorm}]]
- [[EntityManager {typeorm}]]
- [[Repository {typeorm}]]
- [[JoinColumn options {typeorm}]]
- [[save both relations {typeorm}]]
- [[cascade option {typeorm}]]
- [[Bidirectional relationships using inverse relation {typeorm}]]

## 더 알아보기

- [[find options {typeorm}]]
- [Migrations](https://docs.nestjs.com/techniques/database#multiple-databases)
- [[DateColumn Decorators {typeorm}]]
- [[enum column type {typeorm}]]
- [[Validation {typeorm}]]
- [[typeorm 연관 컬럼을 객체가 아니라 id number로 불러오기]]
- indexing
