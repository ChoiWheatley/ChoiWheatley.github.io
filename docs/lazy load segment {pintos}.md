---
aliases: 
tags: 
description:
title: lazy load segment {pintos}
created: 2023-10-13T16:45:13
updated: 2023-10-13T16:59:29
---
- [[week07-10 {swjungle} {pintos}]]
- <https://casys-kaist.github.io/pintos-kaist/project3/anon.html> 참고
___

## `load_segment`와의 차이점

load 안에 `load_segment`를 수행하는데, 파일과 헤더 메타데이터를 `load`로부터 제공받아 유저 프로세스의 세그먼트에 올리는 일을 수행한다. Project1,2에서 사용하는 `load_segment`와 차이가 존재하는데, 이전에는 부지런하게 곧바로 유저 스페이스에 페이지를 할당해 `memset`을 수행했지만 Project3부터는 페이지를 할당하지 않고 `vm_alloc_page_with_initializer`를 통해서 `lazy_load_segment`를 콜백함수의 형태로 등록만 할 뿐이다.

따라서 `load_segment`는 `lazy_load_segment`를 뒤늦게 바인드 하기 위한 작업을 수행한다

> [!important] `install_page`는 오직 project2에서만 쓰는데 현재 코드 복사 과정에서 들어가 있는 것으로 알고있다.

## 호출 타이밍

- [?] page fault 발생시 호출이 될 것이라고 짐작은 하고 있으나 정확히 언제 해야하는지, 어떻게 호출해야하는지에 대한 정보가 없다.
