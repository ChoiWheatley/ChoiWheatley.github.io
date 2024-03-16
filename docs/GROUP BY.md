---
aliases: 
tags: 
description:
title: GROUP BY
created: 2024-03-14T11:47:22
updated: 2024-03-14T14:08:01
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

group by 표현식은 반드시 select 바로 뒤에 오는 컬럼 이름 중에 하나여야만 한다.
