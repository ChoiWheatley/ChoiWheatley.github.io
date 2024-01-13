---
aliases: 
tags: 
description:
title: string_view {C++17}
created: 2024-01-13T12:54:26
updated: 2024-01-14T00:14:27
---
- [[string {C++}]]
- <https://modoocode.com/292#page-heading-9>
---

`const string &` 인자에 스트링 리터럴(`const char *`)을 넣으면 string타입 객체를 암묵적으로 생성하기 때문에 비효율적이다. 따라서 `string_view` 타입이 나왔다.

`string_view`는 문자열의 시작주소와 그 길이만을 저장하고 있기 때문에 string 인자에 비해 효율적이다. 또한 `substr` 연산도 또 다른 view를 리턴하기 때문에 문자열을 수정할 일이 없다면 이것을 쓰는 것이 좋다.

```cpp
void func(std::string_view str) {
	
}
```

<https://stackoverflow.com/questions/46032307/how-to-efficiently-get-a-string-view-for-a-substring-of-stdstring> 에서 string_view가 UB를 낳는 시한폭탄이라는 평가가 있다.
