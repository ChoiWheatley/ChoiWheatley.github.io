---
aliases: 
tags: 
description:
title: JOIN
created: 2024-03-14T11:30:07
updated: 2024-03-14T11:43:36
---
주문과 고객 정보 조합: 각 주문에 대한 주문 번호(orders-orderNumber)와 주문한 고객(customers-customerName)의 이름을 조회하세요.

```mysql
SELECT o.orderNumber, c.customerName
FROM orders AS o
JOIN customers AS c ON o.customerNumber = c.customerNumber;
```

특정 사무실의 직원 목록: 'San Francisco' 사무실에서 근무하는 모든 직원의 이름을 조회하세요.

```mysql
select emp.* 
from employees as emp
join offices on emp.officeCode = offices.officeCode
where offices.city = 'San Francisco';
```
