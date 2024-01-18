---
aliases: 
tags: 
description:
title: namespaces {C++} TODO
created: 2024-01-16T22:29:02
updated: 2024-01-18T18:09:58
---
 - [[C++]]
 - [cppreference.com / namespace](https://en.cppreference.com/w/cpp/language/namespace)
 - [lilys.ai / 포프TV C++ 네임스페이스 헛소리 / AI 요약](https://lilys.ai/digest/165087?sId=phPR4YsoreY&source=video&result=summaryNote&isBlogRequested=false&s=1)  이 영상은 C++에서의 네임스페이스(namespace) 사용에 대한 토론을 다루고 있습니다. 포프가 자바(Java)와 C#에서의 패키지(Package)와 네임스페이스(namespace)를 비교하며, C++에서의 네임스페이스 사용에 대한 불편함을 언급합니다. 그리고 최근에 C++ 17 또는 21에서 한 줄에 여러 개의 네임스페이스(namespace)를 사용할 수 있는 기능을 다루고 있지만 아직 표준화 과정이 진행 중이기 때문에 지원되지 않습니다. 그리고 포프는 C++ 커뮤니티가 언어의 발전 방향을 잘 파악하지 못하고 있는 것에 대해 우려하며, 언어가 발전하는 방향을 무시하고 자신의 원리에 치우치는 사람들이 있다고 지적합니다. 
---

네임스페이스는 위의 요약본과 마찬가지로, 자바의 패키지, C#의 네임스페이스와 동일한 역할을 수행한다. 바로 프로젝트 안에서 이름충돌을 막는 일이다. 

## 사용법

- namespace 선언법
	- `namespace <name> { <declarations> }` : 외부에서 `name::뭐시기` 식으로 사용한다.
	- `inline namespace <name> { <decl> }`: [inline namespaces](https://en.cppreference.com/w/cpp/language/namespace#Inline_namespaces) 는 `using namespace enclosing-namespace` 를 하게 됐을 경우, enclosing-namespace 안에 선언된 인라인 네임스페이스의 모든 선언들이 자동으로 불러와진다. 대표적으로 `std::string_literals`가 인라인 네임스페이스이고, 그 안에 선언된 `operator""s`가 불러와져 바로 쓸 수 있게 된다.
	- `namespace { <decl> }` : unnamed namespace: Translation Unit 안에서만 고유한 전역변수
	- `namespace ns-name :: member-name { <decl> }`: C++17~: 네임스페이스 중첩도 가능하다. `namespace A::B::C { ... }`는 `namespace A { namespace B { namespace C {...}}}` 와 같다.
- namespace 사용법
	- `ns-name :: member-name`
	- `using namespace ns-name`: 많이 써봤지? `std` 쓰기 귀찮아서
	- `namespace name = qualified-namespace`: 네임스페이스 별명도 붙일 수 있다

## inline namespace GPT answer

`inline namespace`는 C++11에서 도입된 기능으로, 라이브러리의 버전 관리나 업그레이드 시 유용하게 사용됩니다. `inline namespace`를 사용하면 라이브러리의 버전을 간편하게 관리하고 호환성을 유지할 수 있습니다. 아래는 간단한 코드 스니펫과 함께 `inline namespace`에 대한 설명입니다.

```cpp
#include <iostream>

// 주 버전
namespace MyLibrary {
    void commonFunction() {
        std::cout << "Common Function in Main Version\n";
    }

    // 인라인 네임스페이스, 버전 1.0
    inline namespace Version1_0 {
        void newFunction() {
            std::cout << "New Function in Version 1.0\n";
        }
    }
}

int main() {
    // 주 버전의 함수 호출
    MyLibrary::commonFunction();

    // 버전 1.0의 함수 호출
    MyLibrary::Version1_0::newFunction();

    return 0;
}
```

이 코드에서:

- `MyLibrary::commonFunction()`은 라이브러리의 주 버전에 속한 함수입니다.
- `inline namespace Version1_0`은 라이브러리의 버전 1.0에 해당하는 인라인 네임스페이스입니다.
- `MyLibrary::Version1_0::newFunction()`은 버전 1.0에서 추가된 함수로, `inline namespace`를 통해 주 버전과 호환성을 유지합니다.

이러한 구조를 사용하면 새로운 기능을 추가하거나 변경할 때, 코드에서 기존 버전과의 호환성을 유지하면서도 새로운 버전을 도입할 수 있습니다. 예를 들어, 새로운 버전이 출시되면 사용자는 필요한 경우 해당 버전의 함수를 사용하거나, `inline namespace`를 통해 주 버전과의 호환성을 유지하면서 새로운 함수를 사용할 수 있습니다.
