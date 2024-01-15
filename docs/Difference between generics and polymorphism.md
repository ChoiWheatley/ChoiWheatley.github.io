---
aliases: 
tags: 
description:
title: Difference between generics and polymorphism
created: 2024-01-14T20:53:38
updated: 2024-01-14T20:53:48
---
- [[0010 Programming 👩‍💻|programming]]
- [What is the difference between Polymorphism and Generics?](https://www.quora.com/What-is-the-difference-between-Polymorphism-and-Generics)
---
답변에 따르면 제너릭은 ‘Parametric polymorphism’, 오버로딩에 의한 다형성은 ‘ad-hoc polymorphism’ 이라고 부른다. 클래스 간 상속으로 동적 디스패치에 의한 다형성은 ‘subtype polymorphism’ 이라고 부른다. 벌써 세 가지 용어가 나왔다.

# Parametric Polymorphism - generic

단순히 코드 재활용성을 극대화한 개념이다. 바이너리가 아닌 코드의 템플릿을 하나 만들어놓고 컴파일 타임에 필요로 하는 서로 다른 타입의 모든 버전을 바이너리에 적재한다.

# Ad-hoc Polymorphism - overloading

ad-hoc 이라는 말 뜻은 다음과 같다.

<aside> 💡 특별한 목적을 위한 ([](http://ko.wordow.com/english/dictionary/ad%20hoc)[http://ko.wordow.com/english/dictionary/ad](http://ko.wordow.com/english/dictionary/ad) hoc)

</aside>

의미가 조금 애매하긴 하다. 여기에선 그냥 ‘오버로딩을 위한’ 으로 대치하여 생각하자.

서로 다른 타입을 다루기 위해서 마련한 분리된 코드를 의미한다.

# Subtype Polymorphism - dynamic dispatch

상속에 의한 동적 디스패치를 제공하는 기능으로, 언어에 따라 가상함수, 덕 타이핑 형태로 제공이 되고 있다.
