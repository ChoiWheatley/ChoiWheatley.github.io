---
aliases: 
tags: 
description:
title: HAVING
created: 2024-03-14T14:11:17
updated: 2024-03-14T14:42:17
---

- [mysql 샘플 데이터베이스](https://www.mysqltutorial.org/mysql-sample-database.aspx)

## 특정 금액 이상의 주문: 

500달러 이상의 총 주문 금액을 기록한 주문들을 조회하세요.

```mysql
select orderNumber
    , sum(priceEach * quantityOrdered) as totalAmount
from orderdetails
group by orderNumber
where totalAmount >= 50000;
```

위 구문은 문법 에러가 발생하게 된다. `where` 쪽에서 생기는 에러인데, 원래 `totalAmount`라는 컬럼이 `orderdetails`라는 테이블에 없기 때문이다.

이때 `HAVING`이라는 키워드를 사용해주어 `SELECT` 결과물 컬럼을 사용하여 **서브쿼리**를 수행할 수 있다.

수정된 코드:

```mysql
select orderNumber
    , sum(priceEach * quantityOrdered) as totalAmount
from orderdetails
group by orderNumber
having totalAmount >= 50000;
```

## 평균 이상 결제 고객:

평균 이상 결제 고객: 평균 결제 금액보다 많은 금액을 결제한 고객들의 목록을 조회하세요.

1. 평균을 구하는 쿼리를 작성한다.
2. 고객별로 결제의 sum을 기준으로 group by한다.
3. 그 고객이 평균보다 높은 지출을 한 경우만 필터링 하여 뽑아낸다

```mysql
select c.customerNumber, c.customerName, sum(p.amount) as totalAmount
from customers c
join payments p on c.customerNumber = p.customerNumber
group by customerNumber
having totalAmount > (select avg(amount) from payments);
```
