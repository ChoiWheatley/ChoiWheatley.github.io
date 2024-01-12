---
aliases: 
tags: 
description:
title: stream, iterator, functional programming {C++}
created: 2024-01-11T17:03:32
updated: 2024-01-11T17:04:39
---
- [[C++]]
- [[C++] 스트림(stream) 클래스 (1)](https://junstar92.tistory.com/461)
- [[C++] 스트림(stream) 클래스 (2)](https://junstar92.tistory.com/462)
---
한글로 깔끔하게 정리된 stream 상속계층도와 인터페이스, 그리고 유용한 전역변수와 함수까지!


스트림을 자바 스트림처럼 정해진 개수 또는 조건만 필터하는 방법은 없을까?

1. `std::cin` 을 `std::istream` 으로 넘겨줄 때 넘겨받는 쪽에서 최대 MAX 개의 바이트만 입력받게 만들게 하고싶다. 그 이상 입력받으려고 하면 더 이상 cin 에서 데이터를 주는 대신 스트림이 닫혀버린다는거.[링크]([https://www.notion.so/stream-iterator-functional-programming-0a865f03062345e78eadbe29a2e84b0a?pvs=21](https://www.notion.so/stream-iterator-functional-programming-0a865f03062345e78eadbe29a2e84b0a?pvs=21))
2. [마찬가지로 넘겨줄 때 내가 원하는 필터를 걸어 받는 쪽에서 그저 스트림을 꺼내기만 하면 되는 거.](https://www.notion.so/stream-iterator-functional-programming-0a865f03062345e78eadbe29a2e84b0a?pvs=21)

---

# std::istream_iterator = istream의 read를 추상화

[std::istream_iterator](https://en.cppreference.com/w/cpp/iterator/istream_iterator)

istream_iterator를 직접 만들어 쓸 일이 있을까 싶지만 여타 이터레이터와 같은 연산을 활용하여 읽기 연산을 수행할 수 있다는 것이 매력적인 것 같다. 기본적으로 설정된 istream_iterator는 eof-stream iterator라고 한다. 따라서 스트림의 끝을 eof로 보는 것이다. 이것을 어떻게 잘 수정하면 원하는 길이만 입력받게 만들 수 있지 않을까?

# filter 등 함수형 프로그래밍

[std::find, std::find_if, std::find_if_not](https://en.cppreference.com/w/cpp/algorithm/find)

Returns an iterator to the first element in the range `[first, last)` that satisfies specific criteria (or last if there is no such iterator):

아직 더 읽어봐야 한다. 하지만 적어도 이터레이터와 Predicate를 지정하면 필터가 가능하다는 것이 c++에서의 함수형 프로그래밍의 희망으로 보인다. 리턴값이 `InputIt` 또는 `ForwardIt` 이런 식으로 이터레이터가 하나밖에 안 빠져나오는데 이걸 어떻게 쓰라는 거지? 그런데 얘들을 전부 템플릿 인자로 만들어 버렸기 때문에 함수 시그니처만 가지고 어떤 역할을 하는 놈인지 알 수가 없다…

 > Return value: 그냥 필터링 하는게 아니라 찾으면 그 위치를 리턴… 내가 원하던 게 아닌데. 물론 이 find_if를 계속 호출하면 결국 원하던 걸 얻을 순 있기는 하다만…

[std\:\:copy_if](https://cplusplus.com/reference/algorithm/copy_if/)

copy_if는 볌위를 돌며 predicate를 만족하는 녀석들만 output iterator에 넣는다.

# for_each, copy + stream_iterator (C++17~)

[How do I use for_each to output to cout?](https://stackoverflow.com/questions/4153110/how-do-i-use-for-each-to-output-to-cout)

- 순회하며 cout에 데이터 넣기

    ```cpp
    std::copy(v_Numbers.begin(), v_Numbers.end(),
              std::ostream_iterator<int>(std::cout, "\\n"));
    ```

- 반복문 없이 cin 입력 데이터를 벡터에 넣기
    
    Conversely, you can copy from a range of `[std::istream_iterator](<http://en.cppreference.com/w/cpp/iterator/istream_iterator>)` into a `vector` using `[std::back_inserter](<http://en.cppreference.com/w/cpp/iterator/back_inserter>)`

    ```cpp
    std::vector<int> v_Numbers;
    std::copy(std::istream_iterator<int>(std::cin), std::istream_iterator<int>(),
              std::back_inserter(v_Numbers));
    ```
