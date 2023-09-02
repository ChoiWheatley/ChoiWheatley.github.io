---
aliases: 
tags: 
description:
title: structures in C
created: 2023-09-02T10:28:30
updated: 2023-09-02T10:36:40
---

## 빵꾸난 지식들

- [[padding bytes in struct {C}]] 까지는 알았는데, 그럼 인스턴스의 멤버를 꺼내기 위해서 진행하는 작업을 뭐라고 부르는지는 생각치도 못했다. 그것은 바로 [Packing](https://www.geeksforgeeks.org/structures-c/)이다. 컴파일러에게 명시적으로 패딩을 하지 말라는 옵션도 있기는 한데 `__attribute((packed))__` 이걸 쓸 일이 올 지는 잘 모르겠다.
- 비트필드를 사용하여 불필요한 패딩 바이트의 삽입을 막을 수도 있다. (하지만 엄청난 혼란이 발생하겠지)

아래의 예시는 비트빌드를 사용해 임의로 정수 멤버의 크기를 4바이트에서 3바이트로 줄인 것을 볼 수 있다.

```C
struct str1 {
	int a;
	char c;
};
struct str2 {
	int a : 24; // size of 'a' is 3 bytes = 24 bits
	char c;
};

sizeof(struct str1); // 4 + 1이상 가장 작은 4의 배수인 8
sizeof(struct str2); // 3 + 1이상 가장 작은 4의 배수인 4
```
