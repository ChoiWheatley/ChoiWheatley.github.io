---
aliases: 
tags: 
description:
title: string_view {C++17}
created: 2024-01-13T12:54:26
updated: 2024-01-14T01:11:00
---
- [[string {C++}]]
- <https://modoocode.com/292#page-heading-9>
---

`const string &` 인자에 스트링 리터럴(`const char *`)을 넣으면 string타입 객체를 암묵적으로 생성하기 때문에 비효율적이다. 따라서 `string_view` 타입이 나왔다.

`string_view`는 문자열의 시작주소와 그 길이만을 저장하고 있기 때문에 string 인자에 비해 효율적이다. 또한 `substr` 연산도 또 다른 view를 리턴하기 때문에 문자열을 수정할 일이 없다면 이것을 쓰는 것이 좋다.

```cpp
/**
 * python-style string slicing
 * str[start:end]
 */
std::string_view slice(std::string_view str, size_t start, size_t end) {
	return str.substr(start, end - start + 1);
}
```

## string_view는 string이 언제 소멸될지 알 수 없어 위험하다

<https://stackoverflow.com/questions/46032307/how-to-efficiently-get-a-string-view-for-a-substring-of-stdstring> 에서 string_view가 UB를 낳는 시한폭탄이라는 평가가 있다.

## 사용예시

<https://leetcode.com/problems/longest-palindromic-substring> 문제 풀이 소스. 파이썬이 개쩌는 언어라는 것을 다시 한 번 깨닫게 만들어주는 문제.

파이썬의 `s[a:b]` 문법은 C++에서 `string_view{s}.substr(a, b - a + 1)`이다.

```cpp
#include <string>
#include <cassert>
class Solution {
    using sv = string_view;
    sv expand(sv s, int left, int right) {
        while (left >= 0 && right < s.size() && s[left] == s[right]) {
            left--;
            right++;
        }
        left++;
        right--;
        return s.substr(left, right - left + 1);
    }

public:
    string longestPalindrome(string s) {
        if (s.size() == 1) {
            return s;
        }
        string_view ret = s; // 'operator=' assgin 연산자도 가능!
        ret = ret.substr(0, 1);

        // 짝수 길이

        for (int i = 0; i < s.size() - 1; ++i) {
            auto palindrome = expand(s, i, i + 1);
            if (palindrome.size() > ret.size()) {
                ret = palindrome;
            }
        }

        // 홀수 길이

        for (int i = 0; i < s.size() - 2; ++i) {
            auto palindrome = expand(s, i, i + 2);
            if (palindrome.size() > ret.size()) {
                ret = palindrome;
            }
        }

        return string{ret};
    }
};
```
