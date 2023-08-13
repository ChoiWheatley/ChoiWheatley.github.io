---
aliases: 
tags: 
description:
title: Doit 자료구조와 함께 배우는 알고리즘 기초 파이썬 편
created: 2023-08-12T12:54:52
updated: 2023-08-13T13:29:45
---
[[0120 swjungle 🤖]]의 일환으로 학습한 자료를 정리한 문서입니다.

- 구조적 프로그래밍: 순차구조(sequential)와 선택(selection)구조, 반복(iteration)구조
	- 순차: 한 문장씩 처리
	- 선택: 조건에 따라 실행 흐름 변경
	- 반복: 조건이 성립하는 동안 주어진 구간의 코드를 반복
		- 카운터용 변수 (`cnt`, `counter`, ...): 반복문을 제어할 때 사용되는 변수
- 형변환 to integer, floating point
	- `int(문자열, 진수)`, `float(문자열)`
- [[헤더와 스위트(suite) {python}]]
- What is the algorithm?
	- 문제를 해결하기 위해 기술된 일련의 실행절차
- Decision Tree (결정트리)
	- 세 정수 a, b, c의 대소관계조합을 모두 나열해보면 트리형태를 띄게 된다. 이것을 결정트리라고 부름.
- DRY (Don't Repeat Yourself) 
	- DRY 메커니즘의 예시: 하드코딩을 피하라 [[reverse를 사용해야 하는 이유 {django}]]
	- 논리적으로 일치하는 분기문을 갖다 쓰지 말라. `if a < b: ... \n elif b >= a: ...` 이딴거 쓰지 말라는 거임 ==p.25==
	- 9999번 A 분기에 들어가고 1번 B 분기에 들어갈거면 if 쓰지 말라. ==p.38==
		-  파이썬은 배열을 각각 리스트와 튜플로 구현할 수 있다. 리스트는 변경이 가능하고, 튜플은 변경이 불가하다는 차이가 존재하다.
		- [?] p.42 ~ p.44 for 문 안에 있는 if문이 무의미한 경우 (1억번 중 1억번 같은 브랜치) 파이썬은 내부적으로 분기예측을 통해 최적화를 하는가? ^8px73d
- `if`구문에서 `else`를 쓰지 않으면 내부적으로 `else: pass`가 숨어있다.
- 연산자와 피연산자, operator, operand. 용어파악
	- 삼항 연산자 (ternary operator) : `x if x < y else y`
- swap: `a, b = b, a` 개쩔지 아니한가 : ~~사실 튜플 공간을 만들어 쓰는거라서... 변수를 하나 생성하여 스왑하는 것보다 비효율적임~~ 튜플은 레퍼런스를 담을 공간이다.
- str `*` 연산자는 문자열을 복제하여 리턴한다.
- 파이썬의 모든 변수는 값을 저장하지 않는다. ==p.56== 
	- 파이썬에서의 모든 건 객체이고, 변수는 그 객체를 참조하는 참조자에 불과. 엄밀한 의미에서의 포인터는 아니지만 고유식별번호가 있어 `id()`를 사용하여 이를 찍어볼 수 있다. 
- [?] 파이썬이 언제 메모리를 비우는가??  
- [?] range 음수 갭을 주었을 때 실험해보자.
___

## Chapter02 기본 자료구조와 배열

- DRY (Don't Repeat Yourself)
	- 배열은 프로그램을 덜 수정하기 위해 도입된 개념이다. ==p.62== 집계, 인덱싱, 정렬, 해싱 등을 활용할 때 세부적이지 않은 사항인 원소의 개수에 영향을 최소화할 수 있다.
- `tuple()`, `list()` 함수를 사용하면 순회 가능한 모든 놈들을 전부 리스트로 만들 수 있다.
- 튜플은 **불변성**에 높은 가치를 두어야 할 것 같다.
- **누적 대입**: `int`, `str` 같은  리터럴들은 값이 변하지 않는다. 1이 갑자기 2가 되거나 하지 않는다는 것이다. 그렇다면 우리가 리터럴을 저장한 변수에 `+=`과 같은 연산을 통해 값이 변하게 만드는 것은 무엇이냐? 단순히 리터럴 1에서 리터럴 2로 참조자를 변경한 것에 불과하다.
	- [!] 그건 그렇고 무한한 정수를 전부 메모리에 담을 수 없는데, 정수형 리터럴은 그럼 일종의 바운더리가 있는걸까? ==> [맞기는 한데, 바운더리의 범위는 IDE나 인터프리터에 따라 얼마든지 달라질 수 있다.](https://stackoverflow.com/questions/63188021/why-python-integer-caches-range-5-256-dont-work-in-similar-way-on-all-platf)
- statement and expression
	- statement: 출력값, 자료형이 없음. `x = 2`와 같은 대입 연산자의 결과는 값을 리턴하지 않는다.
	- expression: 출력값, 자료형이 있음. `x + 17`은 값을 리턴한다. 리턴한 값을 함수 인자에 넣을 수도 있고, statement에 넣을 수도 있다.
	- ![[Pasted image 20230812101702.png]]
- 자료구조 (Data Structure)는 데이터가 모여있는 구조이다. 데이터가 어떻게 모여있는지는 궁금해 하지 않도록 적당히 추상화를 했다. 따라서 자료구조는 추상 자료형 (Abstract Data Type)이다.
- 배열의 대소, 등가관계

	```python
	[1, 2, 3] == [1, 2, 3]
	[1, 2, 3] < [1, 2, 4]
	[1, 2, 3] < [1, 2, 3, 5]
	```

- [`typing.Sequence`](https://docs.python.org/3/glossary.html#term-sequence) : list, [bytes, bytearray](https://dojang.io/mod/page/view.php?id=2462), string, tuple
- [[custom iterator with __iter__ in python]]
- [[call by object reference {python}]]
___
- 소수 나열 ==p.97== 예상시간 15min  
	교재에 나온 방식이 내가 알고 있던것과는 사뭇 다르다. 적어도 책에서는 아리스토테네스의 체를 사용하지 않는다. 수도코드를 작성해보면..

	```python
	primes = [2]
	for n in range(3, MAX, 2):
		for p in primes:
			if n % p == 0:
				break
		else:
			primes.append(p)
	```

for문 끝나자마자 else가 나타나는 것은 처음 봤다. [Why does python use 'else' after for and while loops](https://stackoverflow.com/questions/9979970/why-does-python-use-else-after-for-and-while-loops)

어쨌든 소수를 찾을때엔 `primes`의 모든 원소를 찾아야 하니 $\Pi(N)$정도의 시간이 걸린다. [Prime Counting Function 참고{wiki}](https://en.wikipedia.org/wiki/Prime-counting_function) 조금만 더 최적화를 한다면 굳이 `primes`의 모든 원소를 찾을 필요는 없고, 제곱근 정도의 시간만 소요되게 만들 수 있겠다. 그리고 `MAX`까지 달려갈 때 모든 짝수를 제외할 수 있으므로.... 전체적으로 보았을 때 대충..

$$
O(\frac{\Pi(\sqrt{N})}{2} \cdot \frac{N}{2})=O(N\Pi(\sqrt{N}))
$$

이...렇......게? 되려나? $\Pi$가 대충 로그정도의 속도를 가지고 있다고 하니까 조금만 더 멍청하게 생각한다면 $O(N \log{N})$ 정도.....?? 그냥 에라토스테네스의 체를 사용해서 메모리는 많이 먹어도 $O(N)$에 끝내는 방법이 더 현명해 보이기도 하지만 일단 머리속에 입력은 해두자.

## Chapter03 검색 알고리즘 :: 이진검색 / 이분검색

- 이진검색 ==p.120== 예상시간 30min

[이분탐색 헷갈리지 않게 구현하기 {최백준}](https://www.notion.so/choiwheatley/Binary-Search-fc80010792364e23924f1ec933d467df)

[[binary search를 활용한 lower upper bound 그리고 parametric search까지 {Notion export}]]

파이썬에선 `index()` 함수로 리스트, 튜플 내 원소 검색을 수행할 수 있다.

- meta: 2023-08-12T15:00:41
	- 지금까지 나간 진도, 
		- Ch02 p.61 ~ Ch03 p.130
	- 15:00~17:00 스프린트에서 나갈 진도, 
		- Ch05-03 하노이의 탑 ==p.200==
		- Ch05-04 8퀸 문제 ==p.204==
		- Ch06-1 정렬 알고리즘 ==p.219==
		- Ch06-02 버블 정렬 ==p.221==
	- 오늘 밤까지 끝낼 진도, 
		- Ch06-03 단순선택정렬 ==p.237==
		- Ch06-04 단순삽입정렬 ==p.240==
		- from: Ch06-05 셀 정렬 ==p.247==
		- to: Ch06-09 도수 정렬 ==p.297==
	- 일요일까지 끝낼 진도
		- from: Ch09-01 트리 구조 ==p.377==
		- to: Ch09-02 이진트리와 이진 검색 트리 ==p.382==

## Chapter05 재귀 알고리즘

재귀는 중단조건이 반드시 있어야 한다. 

### 하노이의 탑: ==p.200==

### 8퀸 문제: ==p.204==

[[N-Queen {boj}]]

> 8개의 퀸이 서로를 잡을 수 없도록 8 \* 8 체스판에 배치하시오

[[Z {1074} {boj} {재귀}]]