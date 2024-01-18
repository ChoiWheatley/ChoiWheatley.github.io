---
aliases: 
tags: 
description:
title: enum and enum class {C++}
created: 2024-01-18T22:12:05
updated: 2024-01-18T22:13:38
---
- C++11에 도입

```cpp
// unscoped enum:
// enum [identifier] [: type] {enum-list};

// scoped enum:
// enum [class|struct] [identifier] [: type] {enum-list};

// Forward declaration of enumerations  (C++11):
enum A : int;          // non-scoped enum must have type specified
enum class B;          // scoped enum defaults to int but ...
enum class C : short;  // ... may have any integral underlying type
```

### 매계 변수

- *`identifier:`* 열거자의 기본 형식이며, 모든 열거자는 동일한 기본 형식을 갖습니다. 모든 정수 계열 형식이 가능합니다.
- *`type`: 열거자의 기본 형식이며, 모든 열거자는 동일한 기본 형식을 갖습니다. 모든 정수 계열 형식이 가능합니다.*
- *`enum-list` :* 열거형에서 열거자의 쉼표로 구분된 목록입니다. 범위에 있는 모든 열거자 또는 변수 이름은 고유해야 합니다. 하지만 값이 중복될 수 있습니다. 범위가 지정되지 않은 열거형에서 범위는 주변 범위입니다. 범위가 지정된 열거형에서 범위는 그 자체입니다 *`enum-list`* . 범위가 지정된 열거형에서 목록은 비어 있을 수 있으며 실제로 새 정수 계열 형식을 정의합니다.
- *`class`:* 선언에서 이 키워드(keyword) 사용하여 열거형의 범위가 지정되고 *`identifier`* 반드시 제공해야 합니다. 이 컨텍스트에서 의미상 동일한 키워드(keyword) 대신 **`class`**사용할  **`struct`**  수도 있습니다.

### 열거자 스코프

- 명명된 상수 표현되는 값의 범위를 설명하는 컨텍스트 제공

```cpp
namespace CardGame_Scoped
{
    enum class Suit { Diamonds, Hearts, Clubs, Spades };

    void PlayCard(Suit suit)
    {
        if (suit == Suit::Clubs) // Enumerator must be qualified by enum type
        { /*...*/}
    }
}

namespace CardGame_NonScoped
{
    enum Suit { Diamonds, Hearts, Clubs, Spades };

    void PlayCard(Suit suit)
    {
        if (suit == Clubs) // Enumerator is visible without qualification
        { /*...*/
        }
    }
}
```

### 값 할당 규칙

- case1
    - 따로 할당 값을 지정하지 않으면 좌측을 기준으로 0번부터 1씩 증가한 값 부여

```cpp
// 0, 1, 2, 3
enum Suit { Diamonds , Hearts, Clubs, Spades };
```

- case2
    - 맨 앞의 요소에 대해 할당값이 주어지면 그 값부터 1씩 증가

```cpp
// 1, 2, 3, 4
enum Suit { Diamonds = 1 , Hearts, Clubs, Spades };
```

- case3
    - 여러 요소에 할당값이 주어진 경우는 값이 주어진 요소의 기준부터 순차적으로 증가된 값이 할당되며 만약 증가하다 값이 미리 할당되어져 있는 요소를 다시 만날 시 해당 값부터 다시 증가한다.

```cpp
// 1, 2, 1, 2
enum Suit { Diamonds = 1 , Hearts, Clubs = 1, Spades };
```

- case4
    - 임의의 값을 하나하나 다 할당해둔 경우 해당 값을 가진다.

```cpp
// 4, 3, 2, 1
enum Suit { Diamonds = 4 , Hearts = 3, Clubs = 2, Spades = 1 };
```

## 캐스팅 규칙

- enum Suit { Diamonds, Hearts, Clubs, Spades }; ⇒ non scoped enun
    - 약타입 이므로 암시적 형변환이 가능하다.

```cpp
//int 자료형으로 자동 변환되어 0이 출력된다.
cout << Diamonds;
```

- enum class Suit { Diamonds, Hearts, Clubs, Spades }; ⇒ scoped enum
    - 강타입 이므로 암시적 형변환이 되지 않고 명시적 형변환 필요

```cpp
using enum Suit;
//암시적 형변환이 안되기 때문에 Suit 형은 cout 에서 출력이 불가능 하므로 컴파일 에러 발생
cout << Suit::Diamonds;

//명시적 형변환을 통해 int 자료형을로 캐스팅 하면 출력 가능
cout << (int)Suit::Diamonds;
```

# 강 타입과 약 타입, 정적 타입과 동적 타입

## 강타입

- 강타입은 다른 형끼리 변환이 금지되어 있으며 만약 형변환을 하고 싶다면 명시적으로 타입을 변환시켜줘야 한다.

## 약타입

- 다른 형끼리 변환이 가능하며 암시적 변환 또한 허용하기도 한다.

## 정적 타입

- 컴파일 시에 타입이 결정되므로 자료형을 명시적으로 지정해 줘야 한다.

## 동적 타입

- 런타임에서 타입이 결정되므로 자료형을 명시적으로 지정해지 않아도 작동한다.

![[strong-and-weak-type.png]]

# 열거자가 없는 열거형

- [/std:c++17](<https://learn.microsoft.com/ko-kr/cpp/build/reference/std-specify-language-standard-version?view=msvc-170>)
- 의도하지 않은 암시적 변환으로 인한 미묘한 오류가 발생할 가능성을 제거할 수 있습니다.

## 예시 코드

```cpp
enum class byte : unsigned char { };

enum class E : int {};

//위에서 E : int { e1 = 0 }; 으로 초기화 한것과 같은 결과
E e1{ 0 };
E e2 = E{ 0 };

struct X
{
    E e{ 0 };
    X() : e{ 0 } { }
};

E* p = new E{ 0 };

void f(E e) {};

int main()
{
    f(E{ 0 });
    byte i{ 42 };
    byte j = byte{ 42 };

		cout << (int)e1 << endl;

    // unsigned char c = j; // C2440: 'initializing': cannot convert from 'byte' to 'unsigned char'
    return 0;
}
```

### 링크

[https://learn.microsoft.com/ko-kr/cpp/cpp/enumerations-cpp?view=msvc-170](https://learn.microsoft.com/ko-kr/cpp/cpp/enumerations-cpp?view=msvc-170)
