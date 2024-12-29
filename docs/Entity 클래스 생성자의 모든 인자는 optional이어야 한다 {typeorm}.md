---
aliases: 
tags: 
description: 
links:
  - https://typeorm.io/entities#what-is-entity
status: 
title: Entity 클래스 생성자의 모든 인자는 optional이어야 한다 {typeorm}
created: 2024-12-29T21:55:18
updated: 2024-12-29T22:45:56
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

## 다른 대안은 없나? (GPT)

네, `TypeORM`에서 엔티티 클래스의 생성자에 인자를 넣는 것은 문제가 될 수 있습니다. 이유는 `TypeORM`이 데이터베이스에서 엔티티를 로드할 때 기본 생성자를 사용하여 객체를 초기화하기 때문입니다. 이 과정에서 필요한 인자를 제공하지 않으므로, 위와 같은 `undefined` 참조 에러가 발생합니다.

대안으로 아래의 방법들을 고려할 수 있습니다:

---

### 1. **정적 팩토리 메서드 사용**

정적 메서드를 사용하여 엔티티 인스턴스를 생성하는 방법입니다. 이는 명확한 API를 제공하면서도 ORM이 기본 생성자를 사용할 수 있도록 보장합니다.

```typescript
@Entity()
export class Donation {
  // ...

  @Column()
  amount: number;

  // ...

  private constructor() {
    // 기본 생성자 비공개 처리
  }

  static create(
    funding: Funding,
    senderUser: User,
    amount: number,
    g2gException: GiftogetherExceptions,
  ): Donation {
    const donation = new Donation();

    if (amount > funding.fundGoal) {
      throw new Error("Amount exceeds funding goal");
    }

    donation.amount = amount;
    // 다른 속성 초기화 로직 추가

    return donation;
  }
}
```

이 방법은 객체 생성 시 생성자 대신 `Donation.create()`를 사용하도록 강제합니다.

---

### 2. **데코레이터 기반 유효성 검사 사용**

`class-validator`나 `class-transformer`와 같은 라이브러리를 사용하여 유효성 검사를 수행하는 것도 가능합니다.

```typescript
@Entity()
export class Donation {
  @Column()
  @IsInt()
  @Min(1)
  @Max(10000) // 예시로 최대 금액 제한
  amount: number;

  // ...
}
```

이 방법은 엔티티 속성에 유효성 검사를 적용하고, 비즈니스 로직은 서비스 계층에서 처리합니다.

---

### 3. **서비스 계층에서 검증 로직 처리**

비즈니스 로직을 엔티티에 포함시키지 않고 서비스 계층으로 옮깁니다.

```typescript
@Injectable()
export class DonationService {
  createDonation(
    funding: Funding,
    senderUser: User,
    amount: number,
    g2gException: GiftogetherExceptions,
  ): Donation {
    if (amount > funding.fundGoal) {
      throw new Error("Amount exceeds funding goal");
    }

    const donation = new Donation();
    donation.amount = amount;
    // 속성 설정

    return donation;
  }
}
```

---

### 4. **Lifecycle Hooks 사용**

`@BeforeInsert` 또는 `@BeforeUpdate` 훅을 사용하여 엔티티 생성/수정 시 검증 로직을 추가할 수도 있습니다.

```typescript
@Entity()
export class Donation {
  @Column()
  amount: number;

  @ManyToOne(() => Funding)
  funding: Funding;

  @BeforeInsert()
  validate() {
    if (this.amount > this.funding.fundGoal) {
      throw new Error("Amount exceeds funding goal");
    }
  }
}
```

이 방법은 ORM에서 제공하는 기능을 활용하여 데이터 무결성을 보장합니다.

---

### 요약

가장 적합한 대안은 프로젝트의 아키텍처와 요구사항에 따라 달라집니다:
- **객체 생성 로직이 명확히 필요하다면** 정적 팩토리 메서드를 사용하세요.
- **데이터 검증이 주요 목적이라면** 데코레이터 기반 검증이나 훅을 활용하세요.
- **비즈니스 로직 분리를 원한다면** 서비스 계층으로 이동하세요.

이 중 **정적 팩토리 메서드와 서비스 계층 분리**가 실무에서 가장 많이 사용되는 패턴입니다.
