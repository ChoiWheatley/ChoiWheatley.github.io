---
description:
aliases: 
tags: 
created: 2023-04-07T14:56:25
updated: 2023-07-11T15:21:07
title: Strategy - Rust Design Patterns
---
- https://rust-unofficial.github.io/patterns/patterns/behavioural/strategy.html
- 러스트의 전략패턴은 static하다. 그래도 클로저를 인자로 넣는 방식이 있어 확장성이 용이할 것 같지만 Minimum heap을 사용하는 데 이상한 점이 눈에 띄였다.  
- 무조건 maximum heap이 기본이고, 사용자가 원하는 order로 힙을 구성하고 싶다면 C++와는 다른 방식을 취하게 된다. 바로 Ord를 구현하는 래퍼타입을 넣게끔 하는 것이다. 내 생각엔 그냥 BinaryHeap::with_compare_fn() 식으로 초기화 하는 게 더 좋을 것 같은데 왜 굳이 래퍼타입을 쓰게 만들었을까?
- https://stackoverflow.com/questions/34028324/how-do-i-use-a-custom-comparator-function-with-btreeset 
> Custom comparators currently do not exist in the Rust standard collections. The idiomatic way of doing this is to define a [[Using the Newtype Pattern to Implement External Traits on External Types|Newtype]].