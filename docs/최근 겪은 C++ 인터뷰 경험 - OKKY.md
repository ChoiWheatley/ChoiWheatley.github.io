---
aliases: 
tags:
  - scrap
description: 
title: 최근 겪은 C++ 인터뷰 경험 - OKKY
created: 2024-01-10T15:25:22
updated: 2024-01-10T16:44:04
---
- <https://okky.kr/articles/1482509>
- [[C++]]
---

## Scrapped

안녕하세요.

이번에는 조금 실무적인 글을 써볼까 합니다. C++, 로우레벨 CS지식에 관련한 티키타카 목록입니다. 어차피 불합격해서 물건너간 회사들이고, 회사 여러 군데에서 테크니컬 인터뷰를 보고 받은 질문들을 기억나는대로 일부 선정해서 최대한 문제가 안 되는 선에서 무작위로 섞어서(=회사 범위 희석) 써보고자 합니다.

제가 서류랑 코테를 뚫고 C++ 테크 인터뷰를 본 회사들은 다음과 같습니다. (온라인/오프라인 섞여있습니다.)

-   골동품 가게 주인의 자산증권: Quant Dev
    
-   헛강 트레이딩: Quant Dev
    
-   생각하는 세포: C++ SWE
    
-   기타 자잘한 스타트업 몇 군데
    

이런 글을 쓰게 된 동기부여에는 C++하면 포인터까지만 알고 그 뒤로는 모르는 사람(심지어 포인터만 할 줄 알면 C++ 할 줄 아는 거라고 생각하는 사람들도 있더군요..)이 생각보다 너무 많기도 하고, 개인적으로 정리도 해둘 겸 작성합니다. 아래 글들은 제가 충분히 회사에 대한 정보(누가 어떤 질문을 했는지)를 셔플, 합체, 재분배, 그리고 익명화를 해두었지만, 혹시라도 위 회사들 중 한 군데서라도 공격하는 순간, 그냥 글을 삭제할 수도 있습니다.

___

**1\. 프로그램을 실행했을 때 사용되는 메모리 영역을 몇 가지로 구분하면?**

저는 당시에는 "Code" 영역을 몰라서 3가지만 말했었는데, 그렇게 어려운 내용도 아니지만 그렇다고 중요도가 아주 떨어지는 질문도 아니었던 것 같네요.

**2\. Move semantics 관련해서 이런 코드들을 실행했을 때 copy는 몇 번 발생하고 move는 몇 번 발생하는가?**

제가 위 회사들 중 한 곳에서 이 질문에서 엄청 털렸습니다. Modern C++에서 중요한 토픽 중 하나인데, lvalue, rvalue 그리고 레퍼런스 관련해서 제대로 알아두지 않으면 정확하게 대답하기가 굉장히 어렵습니다. 작정하고 제대로 질문하면 겉핥기로 C++ 공부했다고 외치는 사람들 털어버리기 좋은 주제인 것 같습니다.

**3\. Virtual 함수가 가지는 일반 멤버 함수와의 차이는? 만약 본인이 컴파일러라면 virtual 함수의 실제 binary 구현을 어떻게 했을 것 같은가?**

저는 gcc나 msvc 같은 메이저 컴파일러가 정확하게 virtual semantic을 구현한 방식을 말하지는 못했습니다. 실제로 인터뷰어도 거기까지 기대한 건 아닌 것 같고요. 대신에 알고리즘적인 맥락에서 이러이러하게 구현했을 것 같다고 했고 인터뷰어도 어느 정도 동의하시고 거기서 연쇄 질문을 이어나가셨습니다. 그리고 이러한 구현 방식에서 비롯되는 performance overhead 등도 말하고, 또 그런 주제들에서 연쇄적인 티키타카들도 길게 했던 것 같아요. 그거 말고도 "= 0;" syntax 같은 것도 좀 얘기했고요.

**4\. 본인이 면접을 보기 전까지 진행했던 C++ 관련 개발에서 썼던 modern 기능을 몇 가지 꼽아보자면?**

- [template specialization](https://en.cppreference.com/w/cpp/language/template_specialization)
- [inline specifier](https://en.cppreference.com/w/cpp/language/inline)
	- [translation unit](https://en.wikipedia.org/wiki/Translation_unit_(programming))

저는 여기서 inline variable이랑 template partial specialization을 언급했는데, 막상 inline variable을 언급하고 나니 그게 제대로 동작하는 원리를 잘 몰라서 그 부분은 마이너스였던 것 같고, specialization 관련해서는 이것저것 아는대로 많이 설명했던 것 같아요. 그거 말하니까 "more specialized" semantic이라던가, initialization이 실제로 일어나는 원리 등에 대해서도 연쇄적으로 계속 얘기했던 것 같네요.

**5\. 이러이러한 상황을 구현하기 효과적인 자료구조는 C++ standard library에서 어떤 것들이 있는가?**

- <https://en.cppreference.com/w/cpp/memory#Smart_pointers>

다른 언어가 어떤 지는 잘 모르겠는데, C++는 언어 스펙뿐만 아니라 라이브러리 스펙에 관련해서도 알아야 하는 것들이 많습니다. 스마트 포인터라던가, 기본적인 컨테이너 자료구조라던가, meta-programming 라이브러리라던가, 정말 산더미처럼 쌓여 있죠. 모든 걸 다 알 필요는 없지만 자주 등장하는 일부 컨셉 정도는 알아둬야 한다고 생각합니다.

**6\. 컴파일 타임에서는 언어 스펙에 의해 강제되지만, 런타임에서 약간의 binary code 해킹으로 abusing할 수 있는 것은?**

- ??

이건 인터뷰어가 질문한 건 아니고 제가 답변하면서 신나서 막 떠들다가 나왔던 주제였습니다. 무슨 질문 하다가 이 얘기를 했는지는 기억이 안 나네요.

**7\. new, delete 등으로 동적으로 메모리를 할당할 때 실제 OS에서 어떤 작업이 일어나는가?**

- allocator function `new`를 알아야 하나? <https://en.wikipedia.org/wiki/Allocator_(C%2B%2B)> 
	- general purpose allocator가 존재하나, 몇몇 제약조건을 지킨다는 전제 하에 메모리 풀이나 가비지 콜렉션을 지원하는 커스텀 할당자를 만들어 등록할 수 있다.

이거는 제가 나름대로 답을 제시했는데 인터뷰어가 제가 말한 답이 약간 애매한 정답이라고 하시더군요. OS, compiler 내부 구현에 따라 그런 식으로 구현하는 게 가능은 하지만 항상 그런 건 아니라고.. C++ 인터뷰가 타 언어들 인터뷰랑 확연히 대비되는 부분이 이런 것 같습니다. 프론트 개발자들은 이런 지식이 필요하긴 하겠지만 그것보다 TS 등과 연계된 React, Angular 같은 써드 파티 라이브러리나 브라우저 동작 원리에서 등장하는 컨셉 같은 것들이 더 중요한 경우가 많은 것 같은데, C++ 관련 인터뷰에서는 상당히 로우레벨 시스템적인 질문들이 많이 동반하는 것 같습니다.

**8\. 본인이 써본 버젼보다 더 높은 C++ 버젼에서 upcoming하는 기능이나 standard 라이브러리 등을 아는 게 있다면 써보시오.**

저는 20의 concept, 23의 deducing this를 말했습니다. 아무래도 23은 아직 production-ready는 아니라서 20쪽 질문이 더 많이 나왔던 것 같아요. (다행히도 저도 막 제대로 아는 편은 아니었어서 다행이었던..) concept 얘기하니까 concept의 기능을 concept 등장 이전의 방식으로 구현할 수 있는 방법은 무엇인지, 그거 관련해서 연쇄적으로 나온 질문도 나왔고 그 과정에서 SFINAE 같은 컨셉도 좀 설명했던 것 같고요. 이거 말고도 std::range라던가 thread/async 같은 concurrency 토픽들도 얘기해봄직 하겠죠?

**9\. type alias로 typedef와 using을 사용할 때 차이점은?**

별로 중요한 질문은 아닌 것 같지만 이런 부류의 "자주 쓰지만 디테일은 잘 모를법한" 것들도 물어보더군요.

**10\. const vs constexpr = ?**

이거 관련해서는 길게 얘기는 안 한 것 같은데, 그래도 중요한 토픽이라고 생각합니다. 간단하게 여러 가지 얘기했던 것 같아요.

**11\. C++의 4가지 casting keyword에 대해서 자세히 서술해보시오.**

const\_cast야 쉬우니까 말하고, reinterpret\_cast는 특이하게 사용해본 경험이 남아서 바로 대답했는데, 나머지 2개는 엄청 헷갈렸던 것 같아요. 당시에 제대로 대답 못했던 것 같은데 이것도 중요한 토픽입니다.

**12\. C++의 struct의 멤버가 이런이런 타입이 있을 때 이 struct의 instance 하나가 실제 메모리에서 차지하는 영역은 몇 바이트인가?**

이거 모르면 정말 헷갈립니다. 예외적인 케이스가 있는데 저는 그걸 틀렸습니다. 이 질문하고 직접적으로 관련된 얘기는 아니지만 POD 관련 얘기도 좀 연쇄적으로 했네요.

___

이 외 다른 질문들도 좀 나눴던 것 같은데 기억이 나지 않네요. 그 외 제가 면접관이라면 다음과 같은 사항들도 좀 질문할 것 같습니다.

1.  template의 parameter가 int, bool 등으로 들어갈 수 있는데 non-type parameter를 잘못 사용할 때 발생할 수 있는 문제점들은 어떤 것들이 있는가?
    
2.  Parameter pack에 대해서 설명해보시오.
    
3.  const lvalue ref가 상수값을 할당할 수 있는 이유는?
    
4.  diamond 모양의 dependency 그래프 관계로 클래스 다중상속을 할 때 subclass graph가 어떤 식으로 변하는가?
    
5.  constructor, destructor가 parent class, child class에서 호출되는 순서는?
    
6.  함수 overloading과 template specialization의 차이는?
    
7.  Custom literal을 구현하는 방법은?
    
8.  main 함수가 일반적인 함수들과 다르게 가지고 있는 특별한 제약은 어떤 것들이 있는가?
    
9.  explicit 키워드가 하는 일은?
    
10.  constant pointer와 pointer to constant의 차이는?
    
11.  Production 레벨에서 어지간하면 피해야 할 주요 C++ 코딩 습관은 어떤 게 있는가? (using namespace std; 등)
    

___

C++는 서드파티 application이 아닌 순수 언어 스펙 자체에서 공부해야 할 양이 정말 너무 너무 많은 것 같습니다. Rust도 러닝 커브가 힘든 걸 보면 그냥 로우레벨 언어 종특이 아닌가 싶기도 하고요. C++를 가지고 취업하고자 하는 분들 모두 화이팅입니다.
