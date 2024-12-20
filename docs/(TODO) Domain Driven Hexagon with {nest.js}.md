---
aliases: 
tags: 
description:
title: (TODO) Domain Driven Hexagon with {nest.js}
created: 2024-12-06T18:11:29
updated: 2024-12-20T14:47:47
---
- ref: <https://github.com/Sairyss/domain-driven-hexagon>

---

## README

DDD를 NestJS에 도입하려다 발견한 귀중한 교육자료. README에 도메인 기반 설계 및 개발방법론에 대한 거의 모든 것이 적혀있다. Command, Query 부터 Port, Adapter에 이르는 수직적인 레이어들과 유스케이스, 모듈들로 나뉘어지는 수평적인 레이어들을 모두 다룬다. 팀원들에게 DDD 문화를 도입하기 전에 나부터 정확한 지식을 탑재하고 점진적으로 도메인 주도 개발을 앞장서보자.

## Anemic Models (빈혈 모델)

빈혈 모델이란, 도메인 서비스에 모든 로직이 노출되고 엔티티를 데이터 컨테이너로 사용하는 경우를 일컫는다. 모든 validation과 엔티티 생성, 상태변화가 노출되어 캡슐화와는 거리가 먼 상태이다. 이는 일관적이지 않은 상태를 만들 수 있고, 코드 가독성이 떨어지게 된다. 대표적인 anemic model은 service에 CRUD 기능을 노출한 경우를 들 수 있다.

[[Anemic Model을 피해야 하는 이유]]
