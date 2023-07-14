---
description:
aliases: 
tags: programming/rust 
created: 2023-03-11T19:15:24
updated: 2023-07-11T15:21:09
title: derive attribute
---
<iframe src="https://rust-book.cs.brown.edu/ch05-02-example-structs.html#adding-useful-functionality-with-derived-traits" allow="fullscreen" allowfullscreen="" style="height: 100%; width: 100%; aspect-ratio: 1 / 1;"></iframe>

사용자 정의 타입은 기본적으로 `std::fmt::Display` 트레잇이 구현되어있지 않기 때문에 저시기 하지 않는다. 그래서 우리는 몇가지 선택의 기로에 스게 된다.

1. `std::fmt::Display` 트레잇을 구현하기
2. `Debug` 트레잇을 구현하기

1번의 경우, `println!("{}")` 안의 {} 안에 해당 인스턴스가 들어갈시, 내가 원하는 결과물을 출력하도록 만들어주고, 2번의 경우 오로지 디버그 옵션으로 빌드하였을 때에만 `println!("{:?}")` 안의 `{:?}` 안에 해당 인스턴스가 들어갈시, member들을 자동으로 읽어 출력해주는 기능을 제공한다.

```
add `#[derive(Debug)]` to `Rectangle` or manually `impl Debug for Rectangle`
```

2번을 활용하고 싶으면 위의 메시지를 참고하여 간단하게 `Debug` 특성을 상속받으면 된다. Voila!