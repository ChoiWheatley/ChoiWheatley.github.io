---
description:
title: C++
created: 2023-02-10T16:34:48
categories: 
 - scrapping
 - 스크래핑
 - 스크랩
 - programming language
aliases: 
 - c++
 - cpp
tags: [" programming ", programming]
date created: Friday, February 10th 2023, 4:34:48 pm
date modified: Monday, February 27th 2023, 6:20:45 pm
updated: 2024-01-14T18:120498:55
---
- parent link: [[0010 Programming 👩‍💻|programming]]
- [최근 겪은 C++ 인터뷰 경험 - OKKY](https://okky.kr/articles/1482509) C++ 실무자들에게 자주 묻는 면접질문 모음집
- [pocu.academy C++ 언매니지드 프로그래밍](https://pocu.academy/ko/Courses/COMP3200) 커리큘럼이 좋아서 가져옴
- [modoocode.com 씹어먹는 C++](https://modoocode.com/category/C++) 기초부터 심화, 최신기술까지
- [포프TV C++ 관련 비디오 모음집](https://www.youtube.com/playlist?list=PLW_uvsSPlijtSmrhajc3Y02G86lOieQOb)
---

## Scrapped

- [[lambda는 기본적으로 생성자가 지워져있다.]]
- [[lambda함수가 일반 함수보다 느리다]]
- [[stdout precision and fix]]
- [[const member makes class default ctor deleted]]
- [[pure virtual destructor]]
- [[Functional programming -- expression templates, builds structures representing a computation at compile time]]
- [[람다 캡처 자리에 아예 새 변수를 정의하는 방법]]
- [[optional safe unwrapping]]
- [[owner가 아니라면 raw pointer를 참조해도 괜찮다 - when to use smart pointers]]
- [[pointer ownership in c++]]
- [[Exception Safe Code, Part I - Jon Kalb]]
- [[Value Categories in C++17 - lvalue, rvalue, prvalue]]
- [[enum을 예외로 던지지 말라]]
- [[template 키워드를 선언만 하면 안 되는 이유 C++]]
- [[Google Test]]
- [[CMake]]
- [[google coding conventions]]
- [[typeid]]
- [[gcc options]]
- [[extern 키워드 + 컴파일 인자로 여러개의 cpp 파일을 동일한 스코프에  때려넣자]]
- [[vector나 array는 인접 메모리 공간에 상주하고 있는 변수가 없으면 out of bound error를 일으키지 않는다]]
- [[vector의 back 이나 end는 비어있을 때 undefined behavior를 발생시킨다]]
- [[type casting {C++}]]
	- [[static_cast {c++}]]
- [[Constraints and concepts {C++20}]]
- [[map insert, set insert 잘못 쓰면 인생 망함]]
- [[random {C++}]]
- [[inline {C++}]]
- [[explicit constructor {C++}]]

## C++ 기초

- [[string {C++}]]

## 개꿀팁 (스크랩 완료)

- [[natvis를 활용해 못생긴 vscode 디버거 예쁘고 똑똑하게 탈바꿈하기]]

## C++ 종특 (스크랩 완료)

- [[C++ remainder operation is NOT modulo]]
- [[커스텀 타입을 string으로 변환하는 방법 {C++}]]
- [[stream, iterator, functional programming {C++}]]
- [[references in {C++}]]
- [[소괄호 초기화와 중괄호 초기화의 차이점을 설명해 주세요 {C++11}]]

## Algorithm 개꿀팁

- [[fast io with sync_with_stdio false {C++}]]

## Data Structures with Standard Template Library

- [[unordered_map {{cpp}}]]

## Externel libraries

- <https://github.com/nemtrif/utfcpp> : utf-8을 손쉽게

## 도서

- [Effective Modern C++ 1st Edition](https://www.amazon.com/Effective-Modern-Specific-Ways-Improve-ebook/dp/B00PGCMGDQ?ref_=ast_author_dp&dib=eyJ2IjoiMSJ9.8wycxcWW2JL3mHaDyQLpK0AZJghgeJpe2KVMEsoCPd2LeRj5C2tlwIlfs3uHvqzV-xNEqJUsrudr9KTaUSo3iknni1d5bw2ztr45CEpKg5uS7q9Tg8-N3eFdmAuw_5lMbKo4DwwbzVBXOQpJYrwYroTA25OzL3nfUni1bUUvp1edJjmIBuBzORaZSVOeSPFu.vjrNBOFb5M2QTljaRVksy5GumuXnvSoRwOpaot_0A6A&dib_tag=AUTHOR)

## 질문

- C++ 에서 소리소문없이 생성하는 생성자들 (복사 생성자, move 생성자)과 각 생성자에 대응하는 operator=() 연산자 오버로딩을 꼭 해야하는 걸까?
- 모든 포인터를 shared_ptr로 만들면 될까요? 아니면, 해당 포인터의 owner만 shared 또는 unique로 만들고 그 외엔 raw pointer를 쓰는 게 좋을까요?
- 개수도 추상화 할 수 있나?  `array<T, 3>` 보단 `vector<T>`가 훨씬 추상적이므로 추상 인터페이스를 만들 때 굳이 array로 만들 필요는 없을까?
- const는 값에 사용하는 게 좋을까요, 포인터에 사용하는 게 좋을까요? 링크드 리스트에서 N번째 원소의 값을 바꾸고자 할 때원소를 새 값으로 덮어쓰는 것이 좋을까요, N번째 컨테이너를 RAII 원칙에 따라 새로 생성하고 앞, 뒤를 서로 연결하는 것이 좋을까요?
- [[push_back과 emplace_back의 차이점은 무엇인가요 {c++}]]
- `Base obj = Derive();` 이 문장은 유효한가요?
