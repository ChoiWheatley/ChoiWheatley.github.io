---
aliases: 
tags: 
description:
title: move semantics {C++}
created: 2024-01-19T12:10:27
updated: 2024-01-19T12:59:10
---
- [[C++]]
- [youtube.com / cppcon / back to basics: Move Semantics](https://youtu.be/St0MNEU5b0o?si=W_Te-EuhdfXlyQNk)
---

## std::move

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

**Core Guideline C.66**: 이동생성자를 `noexcept`로 만들어라. 컴파일러 최적화 수행으로 60% 속도향상을 볼 수 있다. throw할 수 있는 연산 안에 이동생성자가 끼어있다면 컴파일러가 안정성을 보장하기 위해 copy를 할 수도 있다고.

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
