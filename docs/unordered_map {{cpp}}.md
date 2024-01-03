---
aliases: 
tags: 
description:
title: unordered_map {{cpp}}
created: 2024-01-03T16:44:57
updated: 2024-01-03T18:06:18
---
- [[C++|cpp]]
- <https://en.cppreference.com/w/cpp/container/unordered_map>
___

## TL;DR

python의 [[dictionary - python]]와 같은 개념이라고 볼 수 있다. Key는 hash 가능해야 한다. 

## Methods

- [at](https://en.cppreference.com/w/cpp/container/unordered_map/at) key와 연관된 value를 찾는다. 없으면 `std::out_of_range` 에러를 발생시킨다
- <a href="https://en.cppreference.com/w/cpp/container/unordered_map/operator_at">operator[]</a> key와 연관된 value의 레퍼런스를 반환한다. 매핑이 없는 경우, insertion을 수행하고 매핑이 존재하는 경우 update를 수행한다.
- [find](https://en.cppreference.com/w/cpp/container/unordered_map/find) key와 연관된 value의 **이터레이터**를 반환한다. 없으면 `end`를 반환한다.
- [contains (since c++20)](https://en.cppreference.com/w/cpp/container/unordered_map/contains) 말 그대로 key 갖고 찾는다.
- [insert](https://en.cppreference.com/w/cpp/container/unordered_map/insert) 컨테이너에 `std::pair<const Key, T>` 타입 쌍을 추가한다.
