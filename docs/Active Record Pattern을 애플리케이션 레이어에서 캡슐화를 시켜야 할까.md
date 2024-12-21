---
aliases: 
tags: 
description:
title: Active Record Pattern을 애플리케이션 레이어에서 캡슐화를 시켜야 할까
created: 2024-12-21T21:10:59
updated: 2024-12-21T21:45:15
---
궁금한게, `FindOrdersByUserIdUseCase` 같이 구현이 한 줄로 끝날 것 같은 단순한 쿼리 스크립트도 꼭 굳이 캡슐화를 해야할까? 게다가 ORM을 쓰는 경우라면 더욱이 Repository 패턴을 자동으로 사용하게 될텐데?

### TL;DR

- 다음 조건에 해당하는 액티브 레코드 패턴들은 캡슐화를 하게 되면 우발적 복잡성이 늘어날 수 있다:
	- 읽기만 수행
	- 비즈니스 로직이 없음
	- 너무 단순해서 한 줄에 끝남
- 단순한 액티브 레코드 패턴들을 다음과 같이 리팩터링하라:
	- Application Service에 읽기전용 연산들을 묶어두라
	- 순수 읽기만 수행하고 비즈니스 로직이 없는 경우, 특히나 ORM을 사용하는 경우, 애플리케이션 계층에서 **직접** 액티브 레코드 패턴 스크립트를 작성하는 것을 허용한다.
	- 때로 읽기 작업이 데이터베이스에 부담이 될때 Query Object를 사용하는 것을 권장한다.
	- TypeORM같이 레포지토리 패턴을 라이브러리 단에서 지원하는 경우, 레포지토리를 상속하여 커스텀 레포지토리를 사용하는 방법을 고안할 수 있다.

### GPT 설전

You're absolutely right—creating a dedicated `UseCase` class for something as simple as `FindOrderByUserIdUseCase` might feel like over-engineering and can lead to unnecessary complexity, often referred to as "accidental complexity." In such scenarios, you can adopt a more pragmatic approach while still respecting the principles of **separation of concerns** and **maintainability**.

Here’s how you can streamline your design:

---

### **1. Combine Simple Queries into Application Services**

Instead of creating a specific UseCase class for a single-line query, you can use an **Application Service** or a shared service class to house such simple, read-only operations.

#### Example:

```typescript
class OrderService {
  constructor(private readonly orderRepository: OrderRepository) {}

  findOrdersByUserId(userId: string): Promise<Order[]> {
    return this.orderRepository.findByUserId(userId);
  }
}
```

- **Benefits**:
  - Reduces the need for multiple trivial UseCase classes.
  - Keeps the focus on the domain when business logic is more significant.
  - Simplifies the codebase without violating SRP (Single Responsibility Principle).

---

### **2. Direct Repository Access in Specific Layers**

In cases where the operation is purely a read query (and doesn't involve business logic), it's acceptable to directly access the repository in the Application Layer, especially if you're using TypeORM.

#### Example:

```typescript
class OrderController {
  constructor(private readonly orderRepository: OrderRepository) {}

  async getOrdersByUserId(req: Request, res: Response) {
    const userId = req.params.userId;
    const orders = await this.orderRepository.findByUserId(userId);
    res.json(orders);
  }
}
```

- **Caveat**: Ensure that this approach is used only for simple queries and does not introduce business logic into controllers or the repository.

---

### **3. Use Query Objects for Read-Only Queries**

For read-heavy operations with potential reuse, a **Query Object Pattern** can abstract the query logic while avoiding full-fledged UseCase classes.

#### Example:

```typescript
class FindOrdersByUserIdQuery {
  constructor(private readonly orderRepository: OrderRepository) {}

  execute(userId: string): Promise<Order[]> {
    return this.orderRepository.findByUserId(userId);
  }
}
```

- This provides a clean interface for queries without overloading the domain or application layers.

---

### **4. Avoid "Obtrusive UseCase Classes" with Generalization**

For trivial use cases, consider grouping similar operations under a more generic application service or use case.

#### Example:

```typescript
class OrderUseCase {
  constructor(private readonly orderRepository: OrderRepository) {}

  findOrdersByUserId(userId: string): Promise<Order[]> {
    return this.orderRepository.findByUserId(userId);
  }

  createOrder(orderData: CreateOrderDto): Promise<Order> {
    const order = this.orderRepository.create(orderData);
    return this.orderRepository.save(order);
  }
}
```

- **When to Apply**: Grouping related operations is a pragmatic choice when individual classes would add unnecessary complexity.

---

### **5. Leverage TypeORM Features**

TypeORM provides convenient ways to simplify your repository logic using decorators and query builders. Use them to streamline queries while keeping domain logic separate.

#### Example:

```typescript
@EntityRepository(Order)
export class OrderRepository extends Repository<Order> {
  findByUserId(userId: string): Promise<Order[]> {
    return this.createQueryBuilder("order")
      .where("order.userId = :userId", { userId })
      .getMany();
  }
}
```

---

### **Key Takeaways**

- Avoid over-engineering trivial UseCases. Use Application Services or group similar functionality.
- For simple queries, direct repository access in the Application Layer is acceptable if kept clean and consistent.
- For read-heavy applications, consider the **Query Object Pattern**.
- Be pragmatic: Overhead can harm maintainability more than it helps with adherence to theory. 

Choose the approach that balances **simplicity**, **readability**, and **scalability**.
