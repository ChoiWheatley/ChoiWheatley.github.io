---
links:
status:
description:
aliases: 
tags: 
created: 2023-05-22T21:37:49
updated: 2024-12-31T23:27:05
title: 0018 Javascript ☕️
---

## 강의자료

- [30분 요약강좌 유튜브 강의와 연관된 {Notion}](https://paullabworkspace.notion.site/2022-30-1-4bc6b655c6054b2db3ad175789ead72b)
- [JS 수업교안 {Notion}](https://www.notion.so/JS-22-6-8723b46e0cde4d90b020b689e5cb9f0a)
- [알잘딱 JS 핵심개념 {Notion}](https://morning-heart-e2a.notion.site/JavaScript-f037c206e538471f9a9f1915b2139a60)
- [JS Deep Dive {GH 제주코딩베이스}](https://github.com/weniv/BackendOrmi/blob/main/JavaScript/%EB%B3%B5%EC%8A%B5.md)
- [자바스크립트 기초 강좌 : 100분 완성 (Youtube 코딩앙마)](https://youtu.be/KF6t61yuPCY?feature=shared)
- [자바스크립트 중급 강좌 : 140분 완성 (Youtube 코딩앙마)](https://youtu.be/4_WLS9Lj6n4?feature=shared)

## 참고하면 좋은 사이트

- 모던 자바스크립트 튜토리얼을 심지어 한글로 제공 | [javascript.info](https://ko.javascript.info/)
- 프론트엔드 개발자의 정석 (모던 자바스크립트) 저자의 강의 사이트 | <https://poiemaweb.com/>

## 기초

- [[JS instance method]]
- [[JS {basic} {types} {for in, for of} {spread} {destructuring}]]
- hoisting과 temporal dead zone
- constructor function
- [[계산된 프로퍼티 {js}]]
- [[Object Methods]]
- [[Symbol 자료형]]

---

## 비동기, Promise, async, await {JS}

> OS레벨에서 스케줄링의 대상은 **프로세스**이지만 JS 레벨에서 스케줄링의 대상은 **함수**이다.

따라서, 콜백함수는, 내가 함수를 원할 때 호출하게 만들기 위한 수단으로 나온 것이다.

> JS에서 기다리는 것은 동시에 할 수 있지만, 처리하는 것은 동시에 하지 못한다.

- [[tasks, microtasks, queues and schedules {js} {TODO}]]
- [[Promise.all]]
	- **Promise.all**은 여러 비동기 작업을 효율적으로 관리할 수 있는 강력한 도구입니다. 병렬로 실행될 수 있는 작업에서 특히 유용하며, 모든 작업의 결과를 한 번에 처리할 수 있습니다. 다만, 하나의 실패가 전체 작업에 영향을 미치므로 이를 방어하기 위한 추가적인 고려가 필요합니다.
- 

---

## 개꿀팁

- [[JS string formatting with positional placeholders --- use backticks]]
- [[range-like feature in Javascript]]
- JS 속성을 어떤 브라우저가 제대로 지원하는지 저장해놓은 사이트 | [caniuse.com](https://caniuse.com/)
- [[response.headers.get(Content-Type) {js}]]
- [[FileReader {js}]]
- [[clear cache in javascript]]
- [[map an array of objects to a dictionary {js}]]
- [[How to load .env file from nodejs]]
- [[object literal to class object using Object.assign {js}]]

## Libraries / Frameworks / Packages

- [[express.js]]
- [[sequelize, a MySQL ORM for javascript]]
- [[0018.4 TypeORM 💾]]
- [[nodemon, auto reload nodejs server {npm}]]
- [[0018.1 Nest.js 🐱]]

## 이호준 강사님의 한마디 (ormi)

> 중고급자 분들은 조금 사정이 다릅니다. 여러분은 연봉 테이블이 높은 곳을 가기 때문에 회사에서 JS를 '당연히' 할 줄 안다고 생각합니다. 할 줄 몰라도 조금만 하면 할 수 있다고 생각하고요. 심지어 Python-Django로 들어갔는데 다음달에는 Node 프로젝트에 투입될 수도 있어요.(물론 회사에서도 공부할 시간은 줍니다.) 자신이 초봉으로 4,000이상의 연봉을 찍을 생각이라면, JS도 어느정도는 깊게 공부하시는 것을 권해드립니다. 눈떠보니 코딩 테스트 전날에 SPA 부분만 한 번 훑어보시고, JS 스나이퍼는 보시면서 깊게 정리 부탁드려요.
