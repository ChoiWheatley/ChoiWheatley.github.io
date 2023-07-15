---
description:
aliases: 
tags: 
created: 2023-05-18T00:11:40
updated: 2023-07-15T23:27:03
title: Multiple Inheritance and mro builtin function
---

<https://www.geeksforgeeks.org/method-resolution-order-in-python-inheritance/>

그거아시져, 마름모꼴로 생긴 상속관계에서 최하단의 클래스가 어느 부모의 attr를 따라야 하는지 충돌이 발생하는 경우 아주 곤란해진다고.  

![[다중상속.excalidraw]]

하지만 파이썬에서는 아주 이상한 방식으로 다중상속 문제점을 해결했고, (좋은 건지는 모르겠지만) 다중상속이 허용된다. 바로 MRO (Method Resolution Order)에 근거한 원칙에 의해 다중상속이 들어가 있더라도 메서드 이름을 찾아가는 데 가장 가까운 관계의 클래스 우선 탐색한다.

`mro`는 `object`의 메서드로, 출력해보면 나의 모든 조상을 리스트의 형태로 출력한다. 그래서 attr 충돌 발생시 `mro`의 왼쪽에서부터 가장 가까운 클래스의 attr를 따라갈 수 있다...
