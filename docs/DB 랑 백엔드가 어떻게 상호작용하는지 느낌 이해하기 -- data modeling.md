---
description:
aliases: 
tags: 
created: 2023-05-26T21:30:57
updated: 2023-07-11T15:21:09
title: DB 랑 백엔드가 어떻게 상호작용하는지 느낌 이해하기 -- data modeling
---
[What is Data Modeling? - IBM](https://www.ibm.com/topics/data-modeling) | 
[[django model]]
[[Data Modeling {book-project}]]

# What is Data Modeling?

데이터 모델링은 비즈니스 로직에 부합하는 자료구조를 설계하여 데이터와 구조 사이의 뚜렷한 연결다리를 놓는일. 또한 다른 데이터타입들과의 관계를 정의하여 그룹화를 수행할 수 있다.

데이터 모델링은 그 추상화 정도에 따라 Conceptual Data Model -> Logical Data Model -> Physical Data Model 순으로 이어지는데

1. Conceptual Data Model은 시스템이 어떤 정보를 저장하게 될지, 어떤 비즈니스 규칙이 적용될지를 정의한다.
2. Logical Data Model은 엔티티로부터 데이터 속성을 추출하게 된다. 데이터 타입, 데이터가 필요한 공간의 크기, 엔티티를 식별하기 위한 관계 등을 알게 된다.
3. Physical Data Model은 Database의 Table을 생성하기 위한 스키마 작성에 돌입하게 된다.

# Glossary

- ER Diagram == Entity Relationships Diagram
- Foreign Key and Primary Key
- Normalization
	- 최승현이 공부하다 포기함
- 