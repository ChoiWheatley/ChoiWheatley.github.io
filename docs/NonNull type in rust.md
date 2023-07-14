---
description:
aliases: 
tags: 
created: 2023-04-09T21:46:25
updated: 2023-07-11T15:21:07
title: NonNull type in rust
---
- https://rust-unofficial.github.io/too-many-lists/sixth-variance.html
- https://doc.rust-lang.org/std/ptr/struct.NonNull.html
- 이 글을 읽은 나는 점심을 나가서 먹을 뻔했다.
- [[Variance in rust|Covariant]]에 대한 내용을 읽어보면 `&mut T`, `*mut T` 타입은 타입 자체가 불변이다. (값이 아니라) 하지만 이는 nullable에 대한 엄청난 니즈를 충족시켜줄 수 없다. 따라서 다음과 같은 괴랄한 구조체가 나왔다.
- 엥? 우리 저번에 [[Too Many Linked Lists]]할 때 `*mut T` 에다가 널을 집어넣지 않았었나?

# NonNull 문서 요약

`*mut T`와 같이 불변타입을 가변타입으로 만들어준다. 원시 포인터를 사용할 때보다 안전한 API를 기반으로 설계되었으며, 심지어 역참조가 일어나지 않는다고 할지라도 Null이 아님을 보장할 수 있다. 만약 정 `NonNull`을 쓰면서 Nullable (...)하고 싶으면 `Option<NonNull<T>>` 이런 식으로 쓰면 된다.

`&T`에 대한 `From` blanket impl이 구현되어있으나, 이걸 가변참조(`as_mut`, `as_ptr`를 `*mut T`로 변환하기)로 쓰면 아주 곤란하다. (당연한거 아닌가)정말 그렇게 쓰고 싶다면 `UnsafeCell`에 래핑하시오.