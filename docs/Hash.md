---
description:
title: Hash
created: 2023-02-16T19:19:11
categories: 
 - algorithms
aliases: 
 - 해시
tags:
  - " algo/hash "
  - algo/hash
  - todo
date created: Thursday, February 16th 2023, 7:19:11 pm
date modified: Monday, February 27th 2023, 6:20:45 pm
updated: 2023-07-11T15:21:08
---
parent link: [[0011 Algorithms ♾️]]

---

# 개념

- Hash function: key를 고정된 길이의 해시값으로 변경해주는 함수. 컴퓨터 내에서 처리되는 모든 데이터는 전부 숫자이다. 따라서 숫자를 가지고 배열의 인덱스에 담으면 된다는 아이디어에서 출발했다. 해시함수의 목표는 **해시 충돌이 거의 나지 않게, 효율적으로 공간을 사용하자** 물론 보안이나 속도적 측면에서도 고려해야 할 사항이 있다.

- Hash Collision: 해시충돌이라고, 서로다른 두개 이상의 키가 하나의 해시값을 얻게 발생하는 문제를 의미한다. 이를 해결하기 위해 Chaining 기법, Open Address Hash 기법을 사용한다.
	- Chaining 기법: 링크드 리스트로 충돌된 데이터들을 일렬로 나열한다. 만약 해시함수의 성능이 정말 더럽다면 최악 $O(N)$ 번 탐색해야 할 수도 있다.
	- Open Address Hash 기법: 해시 충돌 발생시 next n-th 버킷에 담거나 충돌이 일어난 번호에 제곱을 하거나 여분의 해시함수를 더 적용할수도 있다.

- Table 크기를 선택하는 것도 중요한 일이다. 대체로 소수나 2의 거듭제곱을 많이 활용한다. 사실 해시값이 아주 고르게 분포해 있다면 별 문제가 되지 않는다. 하지만 해시값이 너무 몰리거나 특정 수의 배수만 나오는 경우, 공간은 공간대로 써먹지도 못하면서 충돌은 지대하게 많이 나는 경우가 생길 수 있다.
	- 소수 (10007, 20011, 30011, 40009, 100003, 200003이 흔히 쓰이는 소수) 를 사용하면 충돌이 적지만 modulo 연산인 `%`를 사용해야 한다. (느림)

	```cpp
	constexpr size_t TABLE_SIZE = 10007;
	...
	size_t bucket = hash(str) % TABLE_SIZE;
	```

- 2의 거듭제곱을 사용하면 나머지 연산따윈 필요없이 비트마스크 연산을 통해 빠르게 해시값을 버킷넘버로 만들어 낼 수 있다. 하지만 하위 비트를 그대로 사용하기 때문에 중복 확률이 증가한다는 단점이 존재한다. 

	```cpp
	constexpr size_t TABLE_SIZE = 1 << 12;
	constexpr size_t DIV = TABLE_SIZE - 1; 0b1111'1111'...
	...
	size_t bucket = hash(str) & DIV;
	```

# djb2

유명한 문자열 해시함수이다. 문자열을 하나씩 읽어가며 해시값을 정한다. 이때 두 상수 5381과 33이 사용되는데... 

```cpp
unsigned long djbc2(unsigned char * str) {
	unsigned long constexpr hash = 5381;
	int c;
	while (c = *str++) {
		hash = ((hash << 5) + hash) + c; // hash * 33 + c와 동일
	}
	return hash;
}
```

# MySet 구현
 

# 참고
https://woo-dev.tistory.com/106


# 연습문제
- [[문자열 교집합]]
- [[단어가 등장하는 횟수]]
- [[은기의 아주 큰 그림]]
- [[메일서버]]
- [[문자열 암호화]]