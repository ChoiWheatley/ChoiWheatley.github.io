---
description:
aliases: 
tags: 
created: 2023-04-01T23:42:30
updated: 2023-07-11T15:21:07
title: Persistent Data Structures - Hans Enlin
---
- https://blog.hansenlin.com/persistent-data-structures-part-i-the-persistent-list-156f20df3139
- [[Too Many Linked Lists]] 학습중 ch03-persistent stack 장 시작머리부터 이해가 안돼 검색함.
- Confluent Persistent => 합류 지속성, 여러 브랜치의 버전들이 갈라져도 데이터가 오염되지 않는다
- Functional Persistent => 함수 지속성, ??? 모든 노드는 불변이지만 노드의 update, changes들에 대한 정보를 담고 있는 노드를 생성할 수 있다.
- 깃허브 커밋들이 그러면 함수 지속성을 만족하는건가보다. 모든 노드는 이전 노드로부터의 차이점만을 저장하니까. 내가 보는 전체 코드는 그것들의 총합에 불과하다.
- 링크드 리스트와의 극명한 차이점은, 선형적이냐, 트리구조냐에 따른 차이에 있다. 링크드 리스트는 전체 노드를 관장하는 하나의 리스트가 존재하여 노드에 대한 삽입, 수정을 얘 하나가 담당하지만, 지속성 리스트는 모든 노드가 헤드이고 모든 노드의 다음 노드가 꼬리이다. 