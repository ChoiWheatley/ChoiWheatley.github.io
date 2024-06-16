---
description:
aliases: 
tags: 
created: 2023-05-22T21:37:49
updated: 2024-02-15T08:55:55
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
- [[비동기, Promise, async, await {JS}]]
- hoisting과 temporal dead zone
- constructor function
- 계산된 프로퍼티

```js
const user = {
	[expression] : value
}
// same as
user[expression] = value
```

- Object Methods
	- `Object.assign(init, target)`: init 객체에 target 객체를 덮어씌운 새 객체를 리턴
		- `const user2 = user` 이런 식으로 쓰면 안된다. 같은 주소값을 가리키기 때문.
	- `Object.keys(obj)`: 키값만 배열로 반환
	- `Object.values(obj)`: 밸류값들만 배열로 반환
	- `Object.entries(obj)`: 키, 밸류를 가진 이차원 배열을 반환
	- `Object.fromEntries(arr)`: 키, 밸류를 가진 이차원 배열로부터 객체 생성

- Symbol 자료형

JS는 남이 만들어놓은 객체의 프로퍼티를 자유롭게 추가 및 삭제할 수 있다. 그런데 원작자가 의도하지 않은 프로퍼티, 특히나 중복된 이름의 프로퍼티를 추가하려고 한다면 어떻게 될까?

```js
const user = {
	name: "hello",
	age: 28,
};

// 1000 LOC
user.getName = function() {
	return this.name;
};

for (key in user) {
	console.log(key); // name, age, function가 출력됨.
}
```

따라서, 임의 프로퍼티를 안전하게 추가하려면 고유한, 나만이 접근할 수 있는 Key 값으로 새 프로퍼티를 만드는 것이 정신건강에 이로울 것이다. 이를 위해서 만든 개념이 Symbol 자료형이다.

```js
const user = {
	name: "hello",
	age: 28,
};

// 1000 LOC

const getName = Symbol("get name");
user[getName] = function() {
	return this.name;
};

for (key in user) {
	console.log(key); // name, age 까지만 출력됨. getName은 심볼이라서 가려짐
}

user[getName](); // "hello"
```

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
- [[typeorm]]
- [[nodemon, auto reload nodejs server {npm}]]
- [[0018.1 Nest.js 🐱]]

## 이호준 강사님의 한마디 (ormi)

> 중고급자 분들은 조금 사정이 다릅니다. 여러분은 연봉 테이블이 높은 곳을 가기 때문에 회사에서 JS를 '당연히' 할 줄 안다고 생각합니다. 할 줄 몰라도 조금만 하면 할 수 있다고 생각하고요. 심지어 Python-Django로 들어갔는데 다음달에는 Node 프로젝트에 투입될 수도 있어요.(물론 회사에서도 공부할 시간은 줍니다.) 자신이 초봉으로 4,000이상의 연봉을 찍을 생각이라면, JS도 어느정도는 깊게 공부하시는 것을 권해드립니다. 눈떠보니 코딩 테스트 전날에 SPA 부분만 한 번 훑어보시고, JS 스나이퍼는 보시면서 깊게 정리 부탁드려요.
