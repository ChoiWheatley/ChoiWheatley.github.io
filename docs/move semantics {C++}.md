---
aliases: 
tags: 
description:
title: move semantics {C++}
created: 2024-01-19T12:10:27
updated: 2024-01-19T14:48:55
---
- [[C++]]
- [youtube.com / cppcon / back to basics: Move Semantics](https://youtu.be/St0MNEU5b0o?si=W_Te-EuhdfXlyQNk)

	이 영상은 C++의 Move Semantics에 관한 내용이다. Klaus Igelberger는 Move Semantics의 기본 개념, 구현 방법, 그리고 최적화를 소개한다. 이 영상은 Move Constructor와 Move Assignment Operator에 대한 구현 방법을 자세히 설명하고, 오용하지 않고 적절하게 활용하는 지침을 제공한다. 전반적으로 이 영상은 C++ 개발자에게 Move Semantics가 어떻게 동작하는지 이해하고 최적화에 활용할 수 있는 지를 잘 알려준다. 
	
	<https://lilys.ai/digest/174041?sId=St0MNEU5b0o&source=video&result=summaryNote&isBlogRequested=false&s=1>

---

## std::move

static_cast이다, 즉, 런타임에 무조건적으로 l -> r value로 캐스팅한다.

```cpp
std::static_cast<std::remove_reference<T &&>>(l_value);
```

## non-default move ctor & move assignment

<iframe width="560" height="315" src="https://www.youtube.com/embed/St0MNEU5b0o?si=K3u2VcuRHOWNVQAH" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

모든 멤버들이 default move 연산을 제공한다면 "= default"를 붙이거나 아예 암묵적으로 만들어버릴 수도 있으나, 포인터같이 별도로 이동연산을 제공해야만 하는 경우 두개의 연산을 구현해야 한다.

`Self(Self&& w)`, `Self &operator=(Self &&w)`

### Move Constructor

이때 주의해야 할 것이, 모든 멤버들또한 copy가 이루어지지 않도록 move를 수행해야 한다는 것이다. 왜냐면 인자로 들어온 `w`라는 놈은 l-value이기 때문.

```cpp
class Widget {
private:
	int i{0};
	string s{};
	int *pi{nullptr};
public:
	// move ctor
	Widget( Widget && w ) noexcept
		// phase 1: Member wise move
		: i (std::move(w.i))
		, s (std::move(w.s))
		, pi(std::move(w.pi)) 
	{
		// phase 2: Reset
		w.pi = nullptr;
	}
};
```

위 코드에서 i와 pi는 primitive 타입이기 때문에 단순히 복사가 이루어지는 것이 최선이다. 하지만 convention을 위해 명시적으로 move 연산을 박아넣었다. 만약 저 pi라는 놈이 `unique_ptr`라면? default  연산으로 적용가능.

이동생성자 본문에 `w.pi`를 undefined state로 바꿔넣었지만, [`exchange(T& obj, U&& new_value)`](https://en.cppreference.com/w/cpp/utility/exchange) 를 사용하면 pi에는 w.pi를, w.pi에는 nullptr로 바꿔넣을 수 있다.

위의 코드는 **the goal of move ctor** 두개를 모두 지킨다.

1. Transfer the content of `w` into `this`
2. Leave `w` in a valid but undefined state

> [!note] **Core Guideline C.66**: 이동생성자를 `noexcept`로 만들어라. 컴파일러 최적화 수행으로 60% 속도향상을 볼 수 있다. throw할 수 있는 연산 안에 이동생성자가 끼어있다면 컴파일러가 안정성을 보장하기 위해 copy를 할 수도 있다고.

### Move Assignment Operator

```cpp
Widget &operator=(Widget && w) noexcept
{
	delete pi; // explicitly delete pi related memory
	
	i = std::move(w.i);
	s = std::move(w.s);
	pi = std::move(w.pi);

	w.pi = nullptr; // reset

	// 또는 아래와 같이 써도 된다.
	pi = std::exchange(w.pi, nullptr);

	return *this
}
```

move와는 직결되지 않지만 메모리 누수 버그를 없애기 위해 첫줄에 `delete pi`를 하는 모습을 볼 수 있다.

**The goal of move assignment operator**

1. Clean up all visible resources
2. Transfer the content of `w` into `this`
3. Leave `w` in a valid but undefined state

## Special Member Functions

- 기본 move 연산들은 이럴때 생성됩니다:
	- 복사연산이나 소멸자가 사용자 정의되지 않았을 경우 (=default 붙이는 행위도 포함)
	- move 연산이 사용자 정의되지 않았을 경우

> [!note] **Core Guideline C.21**: 기본 연산을 delete하고 싶다면, 필요한 모든 연산을 =delete하라.

## Parameter Conventions

수직으로 분할된 기준은 in-variable, out-variable과 같이 데이터를 어떻게 인자에 넣거나 리턴받을건지 등에 대한 기준이고, 수평으로 분할된 기준은 적혀있다시피 move 속성들을 나타낸다. 여기서 알 수 있는 건 `array<BigPOD>` (Plain Old Data) 은 move를 해도 소용없다는 것이다. 이럴땐 그냥 소유권을 넘기기 보다는 해당 객체를 간접적으로 (레퍼런스로) 작업하거나 읽는 방법이 최선이다.

![[CppCon - Back to Basics Move Semantics (part 1 of 2) - Klaus Iglberger - CppCon 2019 [St0MNEU5b0o - 1313x739 - 53m50s].png]]

## 그래서, POD를 move해, 말아?

[stackoverflow.com / move constructors and static arrays](https://stackoverflow.com/questions/7885479/move-constructors-and-static-arrays)

우선, 복사/이동 생성자, 할당 연산자, 소멸자를 작성할 수 있다면 아예 작성하지 말고, 이러한 것들을 제공하는 고품질 컴포넌트로 클래스를 구성하여 기본 생성 함수가 올바른 작업을 수행할 수 있도록 하라는 일반적인 조언이 있습니다. (역으로 말하면, 이 중 하나라도 작성해야 한다면 아마도 모든 함수를 작성해야 할 것입니다.)

따라서 질문은 "어떤 단일 책임 컴포넌트 클래스가 이동 시맨틱을 활용할 수 있는가?"로 요약됩니다. 일반적인 대답은 다음과 같습니다: 리소스를 관리하는 모든 것입니다. 요점은 이동 생성자/할당자가 리소스를 새 객체에 재배치하고 이전 객체를 무효화하여 (비용이 많이 들거나 불가능할 것으로 추정되는) 새로운 할당과 리소스의 심층 복사를 피할 수 있다는 것입니다.

대표적인 예는 동적 메모리를 관리하는 모든 것인데, 이동 작업은 단순히 포인터를 복사하고 이전 객체의 포인터를 0으로 설정합니다(따라서 이전 객체의 소멸자는 아무 작업도 수행하지 않음). 다음은 순진한 예시입니다:

```cpp
class MySpace
{
  void * addr;
  std::size_t len;

public:
  explicit MySpace(std::size_t n) : addr(::operator new(n)), len(n) { }

  ~MySpace() { ::operator delete(addr); }

  MySpace(const MySpace & rhs) : addr(::operator new(rhs.len)), len(rhs.len)
  { /* copy memory */ }

  MySpace(MySpace && rhs) : addr(rhs.addr), len(rhs.len)
  { rhs.len = 0; rhs.addr = 0; }

  // ditto for assignment
};
```

핵심은 모든 복사/이동 생성자가 멤버 변수의 전체 복사를 수행한다는 것입니다. 이동된 객체가 더 이상 유효한 것으로 간주되지 않고 자유롭게 훔칠 수 있다는 계약에 따라 해당 변수가 리소스에 대한 핸들 또는 포인터인 경우에만 리소스 복사를 피할 수 있습니다. 훔칠 것이 없다면 이동해도 아무런 이득이 없습니다.
