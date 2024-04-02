---
aliases: 
tags: 
description:
title: JoinColumn options
created: 2024-03-26T22:43:11
updated: 2024-04-02T21:08:40
---
- [typeorm.io / JoinColumn Options](https://typeorm.io/relations#joincolumn-options)

`JoinColumn` 데코레이터는 두 테이블의 관계를 **소유**하고 있음을 나타낸다. 관계는 단방향일수도 있고 양방향일 수도 있으나, 소유는 반드시 하나의 테이블만이 가지게 된다. 아래의 예제에서 `PhotoMetadata` 모델은 `Photo`와의 관계를 소유한다. 즉, `PhotoMetadata` 모델이 `Photo` id를 FK로 가지게 된다는 것을 의미한다.

> Relations can be unidirectional or bidirectional. Only one side of relational can be owning. Using `@JoinColumn` decorator is required on the owner side of the relationship.

```ts
@Entity()
export class PhotoMetadata {
	...
	@OneToOne(type => Photo)
	@JoinColumn()
	photo: Photo;
}
```

`JoinColumn` 데코레이터의 인자로 이름을 넣어 임의의 컬럼 이름을 지정해줄 수도 있다.

```typescript
@ManyToOne(type => Catetory)
@JoinColumn({ name: "cat_id" })
category: Category;
```

## referencedColumnName과 name의 차이

- **name**: 내 entity 컬럼이름
- **referencedColumnName**: 연관 entity 컬럼이름
