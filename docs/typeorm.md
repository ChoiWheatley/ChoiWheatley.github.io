---
aliases: 
tags: 
description:
title: typeorm
created: 2023-11-27T16:17:25
updated: 2023-11-27T17:24:45
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

> With TypeORM your models look like this:

[[Column Types {typeorm}{todo}]]

```ts
import { Entity, PrimaryGeneratedColumn, Column } from "typeorm"

@Entity()
export class User {
	@PrimaryGeneratedColumn()
	id: number

	@Column()
	firstName: string

	@Column()
	lastName: string

	@Column()
	age: number
}
```

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

> If you prefer to use the `ActiveRecord` implementation, you can use it as well:

엔티티는 `BaseEntity` 상속만 하면 된다.

```ts
@Entity
export class User extends BaseEntity {...}
```

> And your domain logic will look this way:

```ts
const user = new User();
user.firstName = "Timber";
user.lastName = "Saw";
user.age = 25;
await user.save();

const allUsers = await User.find();
const firstUser = await User.findOneBy({ id: i });
await firstUser.remove();
```

> Let's create our `DataSource`

```ts
import 'reflect-metadata';
import {DataSource} from 'typeorm';
import {Photo} from './entity/Photo';

const AppDataSource = new DataSource({
	type: 'postgres',
	host: 'localhost',
	port: 5432,
	username: 'root',
	password: 'admin',
	database: 'test',
	entities: [Photo],
	synchronize: true,
	logging: false,
});

// call `initialize` method of a newly created database
// ONCE in your application bootstrap
AppDataSource.initialize()
	.then(() => {
		// here you can start to work with your database
	})
	.catch((error) => console.log(error));
```

> Save data with `EntityManager`

```ts
import {Photo} from './entity/Photo';
import {AppDataSource} from './index';

const photo = new Photo();
await AppDataSource.manager.save(photo);
```

> `Repository`를 사용하면 `EntityManager`를 사용하지 않고 같은 짓을 할 수 있음.

[[Repository {typeorm} {todo}]]

모든 엔티티는 해당 테이블에 접근하고 수정할 권한을 가지고 있는 리포지토리 객체에 책임을 전가한다.

```ts
const photoRepository = AppDataSource.getRepository(Photo);
await photoRepository.save(photo);
const savedPhotos = await photoRepository.find()
```

> Let's create a one-to-one relationship with another class. 

`type => Photo` 노테이션은 우리가 연관관계를 맺고 싶은 대상 클래스의 이름을 적는 것으로, `() => Photo` 이렇게 적을 수도 있다.

`JoinColumn` 데코레이터는 두 테이블의 관계를 **소유**하고 있음을 나타낸다. 관계는 단방향일수도 있고 양방향일 수도 있으나, 소유는 반드시 하나의 테이블만이 가지게 된다. 아래의 예제에서 `PhotoMetadata` 모델은 `Photo`와의 관계를 소유한다. 즉, `PhotoMetadata` 모델이 `Photo` id를 FK로 가지게 된다는 것을 의미한다.

```ts
@Entity()
export class PhotoMetadata {
	...
	@OneToOne(type => Photo)
	@JoinColumn()
	photo: Photo;
}
```
