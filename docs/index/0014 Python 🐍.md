---
description:
aliases: 
tags: 
created: 2023-04-12T22:03:42
updated: 2024-09-03T11:08:42
title: 0014 Python 🐍
---
- [[venv activation and deactivation]]
- [[python에서 Optional(Nullable) type을 다루는 법]]
- [python google style guides](https://google.github.io/styleguide/pyguide.html)
- [[0012.1 ESTsoft 백엔드 개발자 부트캠프 오르미 1기 🙊]] 의 일환으로 빡세게 배울 파이썬 강의
- [인프런 알고리즘 베스트 10 - 제코베](https://inf.run/qBQP)

---

# Fast Index

- 강의자료들
	- [[파이썬_부트캠프_얕은물.pdf]]
	- [파이썬_부트캠프_깊은물 - google drive](https://drive.google.com/drive/folders/1HdUC_HOL_evEy1ozrOd2OGcSnozYggSJ?usp=share_link)
	- [Python 기초 - NOTION](https://paullabworkspace.notion.site/Python-c20c452099dc46f4895019df86cb317b)
	- [Python 얕은물 - NOTION](https://shallowpython.notion.site/shallowpython/6e5d012e159d4e3fa3fe6ca8566d9e22?v=b73c91efc98c46e49158156a5927a4fd)
	- [멘토님의 python_deep_dive.ipynb 노트북](https://colab.research.google.com/drive/1IRa8nYwM2HtkzlNJGBlavWK96kffytm_?usp=sharing#scrollTo=E1nDm4EOyOxg)
	- [멘토님의 알고리즘특강 실시간 노트북](https://colab.research.google.com/drive/18ezSWxVCOIRw-AB84gVsap-Zi22oQpkR?usp=sharing)
	- [최승현의 노트북](https://colab.research.google.com/drive/1gxoD01mjta80MkTOlrei1BHSUI0_k9-R?usp=sharing) 
- [[python cheatsheet and snippets]]
- external packages
	- [[0014.5 Web Crawling with Beautiful Soup & requests]]
	- [[0014.4 markdown - python package]]
	- [[0014.3 xlsxwriter - python package]]
	- [[0014.2 pytube 사용한 오픈소스 유튜브 다운로더]]
	- [[0014.1 faker로 더미 데이터 만들기 🐍]]
	- [[0014.6 pymysql, mysql python wrapper]]
	- [[0014.7 loguru, python logging made stupidly simple]]
- Package Managers
	- [[Poetry]]
- useful
	- standard lib
		- [[itertools module]]
			- [[i, j nested for loop with {itertools.product} {python}]]
		- [[re - regex python package]]
		- [[os module (미완성)]]
		- [[dataclasses python module]]
		- [[collections.defaultdict   인덱스 조회 실패 시 디폴트 객체 생성]] 
		- [[collections.Counter, most_common 아이템 개수 계산 {python}]]
		- [[collections.OrderedDict 순서를 보장하는 딕셔너리 {python}]]
		- [[priority queue - python]]
		- [[dataclasses - python -- custom comparator]]
		- [[getattr, setattr {python}]]
		- [[vars 사용하여 파이썬 객체를 dict로 쉽게 변환해보자]]
- test
	- [[unittest module - python]]
	- [[unit tests in python + vscode debugging]]
- read
	- [x] [[flat map in python]]
	- [ ] [[python map function]]
	- [ ] [[Python Type Hints - How to Narrow Types with isinstance, assert, and literal]]
	- [ ] [[understanding python dataclasses]]
	- [ ]  [[typing.Optional and type union in python]]
	- [x] [[self reference a class with typing.Self (python)]]
	- [ ] [[setting up python environment venv requirenemts.txt]]
	- [ ] [[property, getter and setter in {python}]]
 - book
	- [[CPython 파헤치기 - 엔서니 쇼]]
	- [[Fluent Python Clear, Concise and Effective Programming, 2nd Edition]]
	- [[파이썬 알고리즘 인터뷰 - 95가지 알고리즘 문제 풀이로 완성하는 코딩테스트]]
- bootstrap
	- [[check every element from sequence is same {python} {functools.reduce}]]
	- [[sliding window {python} {itertools.islice} {collections.deque}]]
	- [[Comparing result with enum in python]]

---

# Basics of Python (따로뺌)

- [[타입 - python]]
- [[연산자 - python]]
- [[함수 - python]]
	- [[closures, factory functions (python)]]
- [[반복 - python]]
- [[클래스 - python]]
- [[sort - sorted - key - index 추적 - python]]
- [[list comprehension - python]]
- [[lambda - python]]
- [[try - except - else - finally (python)]]
- [[args, kwargs 가변인자와 가변 키워드 인자]]
- [[decorator - python]]
- [[module and package (python)]]
- [[file-io (python)]]
- [[str (python)]]

---

# Daily Document

- [[20230501 estsoft - python - convention, types, variables, int, float]]
- [[20230502 estsoft - python - str,  cpython, indexing and slicing, numeric operations, bit operations, is, not, in]]
- [[20230503 estsoft - python function, list]]
- [[20230504 estsoft - python - sort, deep copy and shallow copy, tuple, dictionary, set, match case]]
- 20230508 if, loop => 걍 수업 안들음
- [[20230509 estsoft - python - list comprehension, try except else, builtin functions, args and kwargs, lambda]]
- [[2023-05-10 estsoft - python - class, class attr and instance attr, magic methods, UserInfo and BookInfo 실습, inheritance]]
- [[2023-05-11 estsoft - python - inheritance, linked-list, method-overriding, MRO, private-member, iterator, generator, module, file-io, excel]]
- [[2023-05-12 estsoft - python]]
- [[20230517 estsoft - python - linked list - dataclass - typing Self cast type union - __getitem__ - slice.indices]]
- [[20230518 estsoft - python - tree -- LIS -- selection sort -- insertion sort -- merge sort -- quick sort]]
- [[20230519 estsoft - python]]

---

# 파이썬 특이사항들

- [[파이썬 함수를 호출할 때 인자 순서를 바꿔써도 된다 + default value]]
- [[match case in python]]
- [[python f-string에서 중괄호 표기하는 방법]]
- [[2023-05-10 estsoft - python - class, class attr and instance attr, magic methods, UserInfo and BookInfo 실습, inheritance#^1eq9ew|__repr__, dir, vars, pprint 관련 개꿀팁]]
- [[custom iterator with __iter__ in python]]
- [[__getitem__ index and slice iteration in python]]
- [[typing.cast은 타입체커에게 정보를 제공한다 - python]]
- [[typing.Callable]]
- [[typing.Iterator]]
- [[`__init_subclass__` Forward declaration of classes {python}]]
- [[python은 분기예측을 하는가 {sof}]]
- [[for else {python}]]
- [[catch multiple exceptions in {python}]]
- `Exception`을 상속하는 클래스는 `raise`가 가능해지는구나 
	- [?] `BaseException`과 `Exception` 간의 차이?
- [[try - except - else - finally (python)]]
- `__contains__`: `in` 문법 사용 가능
- [[dataclass ValueError mutable default class list is not allowed, use default_factory {python}]]
- [[이차원 리스트 전치행렬로 만들기 {python}]]

---

# Python Toy Projects

- [[AutoHotKey Alternative Python Key Mapper]]
- [[Tailwind 기반 HTML 파일 자동생성기]]
- [[0014.1 Django 🎈]]
- [[pyscript]]


---

# 수준 별 공부방향 - 이호준

초급자  
왕도는 없지만, 효율적으로 공부하는 방법은 있습니다.

1. 강사님 colab 보고 다시 코딩 해보기 
2. 이해 안되는 것 손코딩 
3. 개발 블로그에 배운 거 정리 
4. 깃헙 잔디 심기
5. 하루 Lv0 2문제  
(만약 못푸는 문제라면 15분 이상 고민하지 마시고, 답을 손코딩 하는 것을 권합니다.)  
(푼 문제는 깃헙에 잔디를 심기를 부탁드립니다.)  
주말 - Python 얕은물

---
전공자

1. 더 채우려 하지 마시고, 부족한 부분이 어디에 있는가?
2. 일주일 Lv3 ~ Lv4 2문제

주말 - 중급자를 위한 Python + 라이브러리
