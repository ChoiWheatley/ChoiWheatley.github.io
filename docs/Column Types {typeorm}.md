---
aliases: 
tags: 
description:
title: Column Types {typeorm}
created: 2023-11-27T17:15:06
updated: 2024-04-14T23:01:16
---
- [[typeorm]]
- [공식문서](https://typeorm.io/entities#column-types)
___

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
