---
aliases: 
tags: 
description:
title: find options
created: 2024-03-27T00:05:21
updated: 2024-04-14T22:51:24
---
- <https://typeorm.io/find-options>
---

## select

> indicates which properties of the main object must be selected

내가 원하는 컬럼만 선택하여 가져올 수 있습니다. 하기 코드는 SQL 문으로 보자면 `SELECT "firstName", "lastName" FROM "user";`와 같습니다.

```typescript
userRepository.find({
    select: {
        firstName: true,
        lastName: true,
    },
})
```

## relations

> relations needs to be loaded with the main entity. Sub-relations can also be loaded (shorthand for `join` and `leftJoinAndSelect`)

관계는 엔티티와 함께 로드해야 합니다. `Comment` 엔티티에 `funding: Funding` 엔티티에 관계를 가지고 있으므로, `relations: { funding: true }` 옵션을 넣어주어야 합니다.

```typescript
commentRepository.find({
	relations: {
		funding: true,
	}
});
```

## where

> simple conditions by which entity should be queried.

SELECT 문에 들어가는 WHERE절을 자바스크립트 식으로 풀어쓰면 됩니다. 예를 들어 `fundId`와 연관돼 있고 `isDel`이 false인 모든 `Comment` 를 쿼리하는 명령어는 다음과 같습니다:

```typescript
commentRepository.find({
	relations: {
		funding: true,
	},
	where: {
		funding: {
			fundId,
		},
		isDel: false,
	}
})
```

## order

> selection order.

SELECT 문에 들어가는 ORDER 절을 자바스크립트 식으로 풀어쓰면 됩니다. 예를 들어:

```typescript
commentRepository.find({
	relations: {
		funding: true,
	},
	where: {
		funding: {
			fundId,
		},
		isDel: false,
	},
	order: {
		regAt: "ASC",
		comId: "DESC",
	}
});
```

## skip

> offset (paginated) from where entities should be taken.

SELECT의 OFFSET과 동일합니다. Pagination을 수행할 때 사용합니다.

```typescript
commentRepository.find({
	skip: 5,
});
```

## take

> limit (paginated) - max number of entities that should be taken.

SELECT 에서의 LIMIT와 동일합니다. Pagination을 수행할 때 사용합니다.

```typescript
commentRepository.find({
	take: 10,
});
```
