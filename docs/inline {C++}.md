---
aliases: 
tags: 
description:
title: inline {C++}
created: 2024-01-14T01:43:47
updated: 2024-01-14T12:43:11
---
- [[C++]]
---
1. 컴파일 과정을 복습한다.
2. [inline 키워드의 의미](https://modoocode.com/320#page-heading-2) 읽어본다.
    1. Translation Unit (TU)의 의미를 상기한다. 왜 inline 함수는 두개 이상의 TU에서 정의되어 있어도 되는지를 생각한다.
3. 컴파일러가 내놓은 어셈블리 파일을 통해 `inline` 속성에 따라 결과가 어떻게 달라지는지 직접 확인한다.

- TU: 어셈블리 생성단위

## 컴파일 과정 복습

1. 전처리기에 의해 소스코드에 문자열 치환, 헤더 붙여넣기 작업이 수행된다.
2. 컴파일러에 의해 Translation Unit이 생성되고, 이는 곧 목적파일이 된다.
3. 어셈블러에 의해 목적코드가 생성된다.
4. 링커에 의해 목적코드와 외부 정적 라이브러리 파일을 모아서 실행파일 또는 라이브러리가 생성된다.

## One Definition Rule

- [cppreference.com](https://en.cppreference.com/w/cpp/language/definition#One_Definition_Rule)
- [modoocode.com](https://modoocode.com/320#page-heading-1)

> One and only one definition of every non-[inline](https://en.cppreference.com/w/cpp/language/inline "cpp/language/inline") function or variable that is *odr-used* (see below) is required to appear in the entire program (including any standard and user-defined libraries)  
> 
> inline이 아닌 함수 혹은 변수는 전체 프로그램에서 오직 한 번만 정의되어야 한다. (모든 라이브러리르 통틀어)

inline인 변수나 함수의 경우, 이를 사용하고자 하는 TU 안에 정의되어 있어야 한다. 이 제약조건만 지키면 inline 키워드는 여러 군데에서 정의하는 용도로 사용된다.

C++는 헤더파일에 함수의 선언을 써놓고, 소스파일 (.cpp)에 정의를 작성한다. 이 경우 ODR 문제가 발생하지 않지만 만약 헤더파일에 함수의 정의까지 써놓은 경우 inline 키워드를 달아 ODR 규칙을 피할 수 있다!

## GPT 면접예상질문

1. **`inline` 키워드는 무엇이며, 어떤 역할을 하는지 간단히 설명해주세요.**
2. **함수 선언 앞에 `inline` 키워드를 사용하는 것이 좋은 상황은 어떤 경우인가요?**
3. **컴파일러가 `inline` 함수를 어떻게 처리하는지 설명해주세요.**
4. **`inline` 함수와 일반 함수의 차이점은 무엇인가요?**
5. **`inline` 함수로 선언된 함수의 코드가 여러 번 복사되는 문제에 대비한 방법은 무엇인가요?**
6. **클래스의 멤버 함수를 `inline`으로 선언하는 것이 좋은 경우는 언제인가요?**
7. **클래스의 정의와 함께 인라인 함수를 구현하는 것이 좋은가요, 아니면 분리해서 구현하는 것이 좋은가요? 왜 그렇게 생각하나요?**
8. **`inline` 키워드를 남용하는 것이 코드의 성능에 부정적인 영향을 미칠 수 있는 경우는 어떤 경우인가요?**
9. **컴파일러가 `inline` 키워드를 무시할 수 있는 상황은 어떤 경우인가요?**
10. **템플릿 함수에서 `inline` 키워드를 사용하는 것과 관련된 고려 사항에 대해 설명해주세요.**
