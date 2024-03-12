---
aliases: 
tags: 
description:
title: ALTER TABLE 했는데 unique 속성이 사라지지 않는 경우
created: 2024-03-12T18:40:21
updated: 2024-03-12T18:41:06
---
<https://stackoverflow.com/questions/9172164/how-to-remove-unique-key-from-mysql-table>

원인: 인덱스가 사라지지 않아서 그럼

```mysql
ALTER TABLE table
	DROP INDEX column
```
