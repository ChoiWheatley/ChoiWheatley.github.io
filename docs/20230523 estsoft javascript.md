---
description:
aliases: 
tags: 
created: 2023-05-23T11:27:23
updated: 2023-07-15T21:30:22
title: 20230523 estsoft javascript
---

# 상식

JS 생태계 확장의 주역은 바로 [Node.js](https://nodejs.org/en)이다. JS로 백엔드, 시스템, 게임개발 등을 할 수 있게 되었다. 그 전엔 브라우저에서밖에 돌아가지 않는다!

다만, Node를 사용하면 관련 패키지를 엄청나게 많이 설치해야 하는데, 생태계가 그닥 깔끔하지 않다. 속도면에서도 뒤쳐지기도 하고. 노드를 만들던 분이 나와서 [deno](https://deno.com/runtime)를 만들었다.

Babel이라고 하는 트랜스파일러가 존재. (폴리필이라고도 부름) 얘의 역할은 뭐냐? 새로운 제안들에 대한 새로운 표준들을 구 표준에 적용되게 만들어주는 하위호환 backward-compatibility를 가능하게 만들어준다.

npm: 일종의 앱 스토어. 파이썬에선 pip을 쓰듯이 JS 계열에서 가장 많이 쓰이는 패키지 매니저가 npm임.

webpack: HTML/CSS/JS를 하나의 파일로 묶어준다. 장점: 난독화, 최적화 / 단점: 배포할 때 묶어주는 시간(CI/CD가 이를 해결해줌), SEO(검색엔진 최적화)가 매우 큰 단점이었으나 요오즘 크롤링 엔진은 라이브 크롤링을 하여 우리가 눈으로 보는 것을 직접 본다.

# JS 

`window`와 `document`는 ECMA Script 문법이 아니다. 웹 브라우저 객체의 API이다.

`const`, `let`, `var` => `let`과 `const`만 사용하자. 

`var`는 변수 가리기가 허용된다. (shadowing) 뿐만 아니라 `var`는 선언한 순간 전역변수가 된다.

# Type

이거 좀 이상한데

# Instance Method

[[JS instance method]]

![[0018 Javascript ☕️#개꿀팁]]
