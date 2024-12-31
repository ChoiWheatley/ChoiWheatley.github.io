---
aliases: 
tags: 
description:
links:
status:
title: Symbol 자료형
created: 2024-12-31T23:22:11
updated: 2024-12-31T23:22:13
---

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
