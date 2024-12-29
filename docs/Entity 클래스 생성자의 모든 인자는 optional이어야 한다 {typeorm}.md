---
aliases: 
tags: 
description: 
links:
  - https://typeorm.io/entities#what-is-entity
status: 
title: Entity 클래스 생성자의 모든 인자는 optional이어야 한다 {typeorm}
created: 2024-12-29T21:55:18
updated: 2024-12-29T22:01:16
---

## Problem

Entity 클래스에 constructor를 구현하고 인자로 다른 엔티티 클래스를 넣었더니 NestJS 부팅 과정에서 undefined 참조 에러를 발생했다. 아무래도 `DataSource`가 엔티티 클래스의 생성자를 아무 인자 없이 호출하는 것 같은데?

```typescript
@Entity()
export class Donation {
  // ...
  constructor(
    funding: Funding,
    senderUser: User,
	amount: number,
	g2gException: GiftogetherExceptions,
  ) {
    if (amount > funding.fundGoal) { // <<<<<<< 💀💀💀💀💀💀💀💀💀💀💀
      // ... 
    }
  }
}
```

## Reference

> When using an entity constructor its arguments MUST BE OPTIONAL. Since ORM creates instances of entity classes when laoding from the database, therefore it is not aware of your constructor arguments. - [What is Entity](https://typeorm.io/entities#what-is-entity)
