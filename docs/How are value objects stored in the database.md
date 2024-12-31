---
aliases: 
tags: 
description: 
links:
  - https://stackoverflow.com/questions/679005/how-are-value-objects-stored-in-the-database#681106
  - https://thepaulrayner.com/persisting-value-objects/
status: 
title: How are value objects stored in the database
created: 2024-12-31T14:46:08
updated: 2024-12-31T16:08:47
---

## Disclaimer

도메인 주도 설계 원칙에서 "VO를 데이터베이스로 표현하는 방법"에 논의하는 것은 도메인 로직에 세부사항을 포함하여 논점을 흐리는 일이기는 하다. 하지만 적어도 어딘가는 이 과정을 추상화 하고 있으리라 생각이 들기 때문에 파보기로 한다.

> Don't couple logic and domain model to technical representaion - [kravemir](https://stackoverflow.com/questions/679005/how-are-value-objects-stored-in-the-database#comment102863148_681106)

---

VO는 primitive obsession을 방지하기 위해 거의 대부분의 엔티티 구성요소에 들어간다. 예를 들어 RGB 컬러를 `rgb_r`, `rgb_g`, `rgb_g`이렇게 세개의 숫자형으로 표현하는 것은 가독성을 해칠 뿐만 아니라 사용하는 쪽에서 매번 validation을 수행해야 하는 문제가 발생한다.

엔티티, 애그리게이트의 모든 프로퍼티를 VO로 선언하게 되면 아래와 같이 표현이 될 듯 하다. 이 애그리게이트를 데이터베이스에 저장하기 위해선 결국 primitive type으로 변환될 필요가 있다.

```typescript
class Order {
	orderId: OrderId; // -> bigint
	requestUserId: UserId; // -> bigint
	total: Money; // -> { amount: bigint, currency: varchar }
	receiveAddress: Address; // -> { zipcode: varchar, road: varchar, detail: varchar }
}
```

내가 알기론 메모리에 존재하는 인스턴스를 데이터베이스로 보내기 위한 작업도 Serialization으로 부르는 것 같은데 동의어로 Dehydration이라고도 부르는 것 같다. [[serialization, deserialization, 다양한 유의어들]]

DB는 (당연하게도) VO 타입에 관해 전해들은 바가 없기 때문에 위의 `Order` 클래스를 곧장 Serialize 할 수 없어보인다. 따라서 어떤 변환 과정이 그 중간에 포함되어 있을 것으로 기대한다.

**예상1. 테이블을 더 둔다**

`Money`, `Addres`와 같이 여러 추가적인 정보들이 들어가야 하는 VO들을 아예 별도의 테이블로 취급한다. VO도 데이터베이스 테이블로 간주할 수 있다. 물론 식별자는 필요하지 않을 것이다.

```
-- Order 테이블
OrderId | RequestUserId | MoneyId | AddressId
------------------------------------------------

-- Money 테이블
MoneyId | Amount | Currency
----------------------------

-- Address 테이블
AddressId | Zipcode | Road | Detail
-----------------------------------
```

이 경우의 단점이라면 JOIN 횟수 증가로 인한 DB 쿼리 성능감소가 있을 것 같다. 항상 SELECT 할 때마다 JOIN을 하게 될 것이다.

**예상1.1. 반정규화해서 하나의 테이블에 컬럼을 몽땅 저장한다**

JOIN으로 인한 성능하락을 최소화하기 위한 전략은 `Money`, `Address`등을 전부 풀어헤쳐서 같은 테이블의 컬럼으로 넣어버리는 것이다. 쿼리할때 VO를 위한 JOIN을 할 필요가 없다는 점이 장점이지만... 이번에는 Serialization에 오버헤드가 들어갈 지도 모른다는 생각이 든다.

```
OrderId | RequestUserId | Amount | Currency | Zipcode | Road | Detail
---------------------------------------------------------------------
```

**예상2. TEXT 타입으로, Json 혹은 csv 타입으로 저장했다가 불러올때 split한다**

TypeORM의 자체 기능으로 [`simple-json`](https://typeorm.io/entities#simple-json-column-type) 타입이 있다. 이 타입은 저장할 땐 단순히 JSON 포맷의 문자열로 저장하고, 불러올 때 자체적으로 `JSON.stringify` 함수를 호출하여 JS Object로 변환해준다.

|OrderId | RequestUserId | Money           | Address |
|-------|----------------|--------------|--------------|
|1       | 101           | {"amount": 10, "currency": "USD"} | {"zipcode": "12345", "road": "Main St", "detail": "Apt 101"}

## **새로운 대안1. PostgreSQL의 JSONB 타입을 사용한다**

PostgreSQL이 지원하는 **JSON**(혹은 JSONB) 타입은 작은 크기의 VO들을 저장하는 데 매우 유용할 수 있습니다. 이를 사용하는 것은 특정 상황에서 큰 장점이 있지만, 몇 가지 주의할 점도 있습니다. 아래에서 PostgreSQL의 JSON 타입을 사용하는 장단점과 사례를 분석해보겠습니다.

---

### **PostgreSQL JSON(JSONB) 타입 저장 방식**

작은 VO를 JSON으로 저장하면 아래와 같이 엔티티의 특정 필드에 VO 전체를 JSON 형태로 직렬화하여 저장할 수 있습니다.

```sql
CREATE TABLE orders (
    order_id BIGINT PRIMARY KEY,
    request_user_id BIGINT NOT NULL,
    total JSONB NOT NULL, -- { "amount": 1000, "currency": "USD" }
    receive_address JSONB NOT NULL -- { "zipcode": "12345", "road": "Main St", "detail": "Apt 101" }
);
```

### **장점**

#### 1. **VO 저장 및 재사용의 간편성**

- VO를 별도의 테이블로 분리하지 않아도 되며, 여러 필드를 하나의 JSON으로 저장할 수 있습니다.
- VO를 그대로 저장하므로 직렬화 및 역직렬화 과정에서 코드를 단순화할 수 있습니다.

#### 2. **스키마 유연성**

- JSON 타입은 스키마를 엄격하게 강제하지 않으므로, VO의 구조가 자주 변경되더라도 테이블 스키마를 수정할 필요가 없습니다.

#### 3. **PostgreSQL의 JSONB 기능**

- PostgreSQL의 `JSONB`는 효율적인 바이너리 형식으로 저장되어, JSON 데이터를 인덱싱하고 검색할 수 있습니다.
- JSON 속성에 대해 쿼리를 실행할 수 있습니다. 예를 들어:

```sql
SELECT * 
FROM orders 
WHERE total->>'currency' = 'USD' 
  AND total->>'amount'::BIGINT > 100;
```

- 특정 키에 대한 인덱스를 생성할 수도 있습니다.

```sql
CREATE INDEX idx_total_currency ON orders USING gin ((total->'currency'));
```

#### 4. **작은 VO 처리에 적합**

- 작은 크기의 VO(예: Money, Address 등)에 적합하며, 복잡한 JOIN을 피할 수 있습니다.

---

### **단점**

#### 1. **쿼리 성능**

- JSONB는 효율적이지만, 원시 컬럼에 비해 검색 성능이 약간 저하될 수 있습니다.
- 대규모 데이터에서 JSON 필드를 자주 조회하거나 업데이트하는 경우 성능 이슈가 발생할 수 있습니다.

#### 2. **데이터 일관성**

- VO의 특정 필드에 대한 제약 조건(예: NOT NULL, UNIQUE)을 데이터베이스 레벨에서 강제하기 어렵습니다.
- 예를 들어, `Money` VO의 `currency` 필드가 특정 값만 가지도록 하려면 애플리케이션 코드에서 검증해야 합니다.

#### 3. **VO 간 연관 관계**

- JSON으로 저장된 VO 내부의 필드를 다른 엔티티와 조인하거나 참조하기 어렵습니다.
- 복잡한 관계형 데이터 모델링에는 적합하지 않을 수 있습니다.

#### 4. **읽기/쓰기 비용**

- JSON 데이터를 읽고 쓸 때 직렬화 및 역직렬화 비용이 발생합니다.
- 데이터가 너무 크면 이런 비용이 누적될 수 있습니다.

---

### **추천 전략**

- **작은 VO라면 JSONB를 기본적으로 고려**: VO가 독립적이고 다른 테이블과 연관이 적다면 JSONB로 처리하는 것이 효율적입니다.
    
- **복잡하거나 빈번한 검색이 필요하면 정규화**: 검색, 필터링, 정렬이 자주 필요한 필드는 정규화를 통해 개별 컬럼으로 분리하세요.
    
- **JSONB와 정규화 혼합 전략**:
    
    - 자주 조회되는 핵심 필드는 테이블 컬럼으로 두고, 덜 중요한 VO 데이터는 JSONB로 저장하는 방식도 가능합니다.

```sql
CREATE TABLE orders (
    order_id BIGINT PRIMARY KEY,
    request_user_id BIGINT NOT NULL,
    amount BIGINT NOT NULL, -- Money의 핵심 데이터
    currency VARCHAR(3) NOT NULL,
    receive_address JSONB NOT NULL -- Address 전체를 JSON으로 저장
);
```

이렇게 하면, 주요 필드는 효율적으로 검색 가능하면서도 VO의 나머지 데이터를 JSON으로 저장해 유연성을 확보할 수 있습니다.

---

PostgreSQL JSON 타입을 사용하면 설계가 간결해지면서도 유연한 데이터 모델을 구축할 수 있습니다. 위 내용을 바탕으로 프로젝트의 요구사항에 맞는 설계를 선택하시면 됩니다! 😊
