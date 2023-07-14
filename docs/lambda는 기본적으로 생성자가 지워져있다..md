---
description:
aliases: 
tags: 
date created: Friday, February 10th 2023, 4:37:03 pm
date modified: Monday, February 27th 2023, 6:20:45 pm
created: 2023-02-10T16:37:03
updated: 2023-07-11T15:21:08
title: lambda는 기본적으로 생성자가 지워져있다.
---
link: [[C++|c++]]
___
[[c++ - Understanding how Lambda closure type has deleted default constructor - Stack Overflow|Understanding how Lambda closure type has deleted default constructor]]

https://en.cppreference.com/w/cpp/language/lambda 에 따르면...

> Closure types are not [DefaultConstructible](https://en.cppreference.com/w/cpp/named_req/DefaultConstructible "cpp/named req/DefaultConstructible"). Closure types have no default constructor. [^1]

클로저 타입은 기본 생성자가 "없"습니다.

> If no captures are specified, the closure type has a defaulted default constructor. Otherwise, it has no default constructor (this includes the case when there is a capture-default, even if it does not actually capture anything). [^2]

클로저 타입에 캡쳐가 하나도 없다면, 기본 생성자를 가질 수 있습니다.

___

[^1]: until c++20
[^2]: since c++20