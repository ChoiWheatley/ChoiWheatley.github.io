---
aliases: 
tags: 
description: 
links:
  - https://en.wikipedia.org/wiki/Serialization
status: 
title: serialization, deserialization, 다양한 유의어들
created: 2024-12-31T15:03:31
updated: 2024-12-31T15:17:33
---
Serialization은 메모리에 상주한 객체를 저장이 가능한 포맷 (JSON, XML, DB 등) 또는 네트워크로 전송하기 위한 포맷 (Data Stream, Data Buffer, Byte Stream, Url encoding, Base64 등)으로 변환하는 프로세스를 지칭한다. 

- Pickling
- Marshalling
- Dehydrating

---

반대로 네트워크로부터 들어오는 스트림 또는 저장된 포맷으로부터 데이터를 추출해 객체에 채우는 과정을 Deserialization으로 부른다.

- Unmarshalling
- Hydration <https://stackoverflow.com/questions/6991135/what-does-it-mean-to-hydrate-an-object>
	- Deserialization과는 느낌이 조금 다른 것 같은데, lazy-loading의 특성이 포함되어 있는 것 같다. 즉, 객체를 생성만 하고 있다가 access 요청이 들어오면 그때 데이터를 추출해 객체에 채우는 과정을 Hydration이라고 부르는 것 같다. 
