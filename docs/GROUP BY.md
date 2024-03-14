---
aliases: 
tags: 
description:
title: GROUP BY
created: 2024-03-14T11:47:22
updated: 2024-03-14T11:52:55
---
제품 라인별 제품 수: 각 제품 라인에 속한 제품의 수를 조회하세요.

```mysql
select productLine, count(*) as productCount
from products
group by productLine;
```

| productLine      | productCount |
| ---------------- | ------------ |
| Classic Cars     | 38           |
| Motorcycles      | 13           |
| Planes           | 12           |
| Ships            | 9            |
| Trains           | 3            |
| Trucks and Buses | 11           |
| Vintage Cars     | 24           |
