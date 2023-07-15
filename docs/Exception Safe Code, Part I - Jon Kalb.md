---
description:
aliases: 
tags: 
created: 2023-04-12T22:35:13
updated: 2023-07-15T21:33:05
title: Exception Safe Code, Part I - Jon Kalb
---
c++에서 exception은 때론 찾기 힘든 버그를 만들어내곤 한다고 들었다. 예를 들어 구글의 코딩 스탠다드에 보면 C++ exception을 사용하지 않는다고 한다. 어떤 코드에서 throw를 한다면 얘를 호출하는 수많은 함수들이 전부 exception saftey를 보장해야 하는 점이 대표적이라고.  
  
그럼 아주 얕은 함수호출에 대한 exception은 사용해도 좋은가? 또는 특수성이 보장되는 상황에서의 exception, 예를 들면 B+tree에서 LeafNode였던 루트노드가 NonLeaf가 되어야 하는 상황  
  
라이브러리를 만든다고 친다면 라이브러리 안에서 자기 안에서 일어나는 예외를 전부 처리하고 사용자 입장에서 예외를 넘기지 않는다면 안전한 거 아닌가?
<iframe title="CppCon 2014: Jon Kalb &quot;Exception-Safe Code, Part I&quot;" src="https://www.youtube.com/embed/W7fIy_54y-w?feature=oembed" height="113" width="200" allowfullscreen="" allow="fullscreen" style="aspect-ratio: 1.76991 / 1; width: 100%; height: 100%;"></iframe>

- Think structrually  
- Maintain invariants  
- Exception Safety Guarantees 꼭 염두하기 : 오염되지 않는 변수, 자원 유출 방지. "Do or do not", 명시적으로 실패하지 않는 operation 정의하기
