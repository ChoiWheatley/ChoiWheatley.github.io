---
aliases: 
tags: 
description:
title: POST와 DELETE의 차이
created: 2023-06-10T11:41:00
updated: 2023-07-15T21:33:03
---
- 멱등성의 차이.
	- POST가 멱등성이 없다. => 서버의 상태를 계속 변경하다.
	- DELETE는 멱등성이 있다. => 여러번 수행해도 한 번만 수행한 것과 같다.
