---
description:
aliases: 
tags: 
created: 2023-03-16T17:02:38
updated: 2023-07-11T15:21:07
title: 러스트로 game of life 구현하기
---
- https://yceffort.kr/2022/04/rust-wasm-project-tutorial-2 [[0080 Scraps 📚]]
- [rustwasm game-of-life tutorial](https://rustwasm.github.io/docs/book/game-of-life/setup.html) [[0080 Scraps 📚]]
- move to notion https://choiwheatley.notion.site/game-of-life-e379dd1fbf1644dd9997f1c9731c4f00

# What will I learn?

- 러스트 코드를 WA로 컴파일하는 방법을 터득한다.
- Rust, WA, JS, HTML, CSS를 사용하며 폴리글랏 프로그램을 개발할 수 있다.
- Rust와 WA의 강점, JS의 강점을 모두 극대화한 API를 디자인 할 수 있다.
- Rust로 컴파일된 WA 모듈을 디버깅 할 수 있다.
- 프로파일을 통해 프로그램을 더 빠르게 만들 수 있다.
- `.wasm` 바이너리 파일의 크기를 더 작게, 더 빠르게 만들어 빠른 네트워크 로딩을 달성할 수 있다.


# [create-wasm-app](https://github.com/rustwasm/create-wasm-app)

rustwasm 패키지를 webpack에 담가주는 템플릿. 단순히 내가 배포하려는 패키지 루트에서 다음 명령어를 실행하면 된다.

```sh
npm init wasm-app www
```

incremental build를 위해서 사전 작업을 수행했다. `www/package.json` 파일에 루트 폴더의 wasm 바이너리가 들어있는 폴더를 의존하도록 만들었다.

```json
...
"dependencies": {
	"game-of-life-wasm": "file:../pkg"
}
```

그리고 `index.js` 파일에 placeholder로 달려있던 임포트 구문을 자연스럽게 `game-of-life-wasm` 으로 바꾸어 주었다.

```js
import * as wasm from "game-of-the-life-wasm";

wasm.greet();
```

`npm install`, `npm run start` 명령으로 간단하게 웹을 띄울 수 있다.

![[Pasted image 20230316230116.png]]

# Interfacing Rust and JS

WA는 단지 선형 메모리 공간을 JS에게 제공할 뿐이다. WA는 반대로 JS가 점유하는 힙 영역의 공간을 참조할 수 없다. 상당히 제약적인 것이다! JS는 오직 WA의 배열버퍼(`u8`, `i32`, `f64`, 등)만 가지고 WA의 메모리 공간을 참조할 수 있다. WA 함수는 오직 스칼라 값들만 리턴할 수 있다. WA는 JS의 전역 객체를 참조할 수 있다고 한다. [[js-sys crate]]를 이용하면 된다고..

`wasm_bindgen` 특성은 복합 구조체를 두 경계 간에 넘나들게 만들어주는 공통적인 이해관계를 제공해 준다. Rust 구조체를 Boxing 하고, 그 포인터를 JS 클래스에 래핑하여 유용하게 만들어주기도 하고 JS 객체의 테이블 인덱스로 만들어주기도 한다. 

1. 불필요한 복사는 오버헤드를 일으킨다. => WA 선형메모리의 데이터를 복사하는 일을 줄여라!
2. 직열화와 역직렬화는 오버헤드를 일으킨다. => 큰 데이터 객체를 주고받는 일이 없도록 하라! 
	1. opaque handle (불투명 핸들?)을 넘겨주면 JS에서 원격으로 WA 메모리 공간을 사용할 수 있어 오버헤드가 줄어든다.

# Game of life를 구현하기 위한 인터페이스

2D Universe는 일차원 배열로 길게 만들 수 있다. 다음과 같이. 

![[Pasted image 20230323184722.png]]

처음 방식은 `std::fmt::Display for Universe` 를 구현함으로써 단순 텍스트로 변환된 결과를 리턴해 볼 것이다.