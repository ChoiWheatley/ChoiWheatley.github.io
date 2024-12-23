---
links:
  - http://boj.kr/23567
status: 
description: 
aliases: 
tags:
  - two_pointers
created: 2023-04-15T13:37:12
updated: 2024-12-23T18:43:17
title: Double Rainbow boj 23567
---
- <http://boj.kr/23567>
- 최적화 문제 : 가장 작은 길이의 P'을 구하기 => 길이 `l`짜리 P'가 존재하는지?
- 아니다, [[two pointer]]였다. 
- [참고한 블로그](https://seoul-doggy.tistory.com/18)
- 

## Source Code

```cpp
#include <algorithm>
#include <cstdint>
#include <iterator>
#include <vector>

#define cr const &

using std::vector;
using Vec = vector<int>;

constexpr size_t INV = (1 << 30) - 1;

inline bool is_rainbow(Vec cr sequence) {
  return std::all_of(sequence.begin(), sequence.end(),
                     [](auto e) { return e > 0; });
}

template <typename Iter> inline bool try_advance(Iter end, Iter &dst) {
  if (std::next(dst) != end) {
    std::advance(dst, 1);
    return true;
  }
  return false;
}

/**
@brief: two pointers

The problem gurantees that given sequence has at least one
point which is colored with `i` where 0 <= i < k

*/
inline size_t solution(Vec &&ls, size_t k) {
  const auto BEGIN = ls.begin();
  const auto END = ls.end();
  for (auto &e : ls) {
    e -= 1; // normalize
  }
  // initial registration of sequences,
  // b_inner starts in range [0, 0), which is empty and
  // b_outer starts in range [0, n), which has all
  auto b_outer = Vec(k);
  auto b_inner = Vec(k);
  for (auto cr e : ls) {
    b_outer[e] += 1;
  }

  size_t ret = INV;
  auto lo = BEGIN;
  auto hi = lo;

  while (lo != END && hi != END) {
    if (is_rainbow(b_inner)) {
      if (is_rainbow(b_outer)) {
        // @@@@@@ DOUBLE RAINBOW @@@@@@
        auto len = std::distance(lo, hi);
        ret = std::min(ret, size_t(len));
        // Because much shorter answer might exists, keep going through
        // with advancing `lo` iterator
        b_inner[*lo] -= 1;
        b_outer[*lo] += 1;
        if (!try_advance(END, lo)) {
          goto EARLY_RETURN;
        }
      } else {
        // feed b_outer with advancing `lo` iterator
        b_inner[*lo] -= 1;
        b_outer[*lo] += 1;
        if (!try_advance(END, lo)) {
          goto EARLY_RETURN;
        }
      }
    } else {
      // no rainbow found, feed `b_inner` with advancing `hi` iterator
      b_inner[*hi] += 1;
      b_outer[*hi] -= 1;
      if (!try_advance(END, hi)) {
        goto EARLY_RETURN;
      }
    }
  }

EARLY_RETURN:

  if (ret == INV) {
    // no double rainbow found
    return 0;
  }
  return ret;
}


#include <ios>
#include <iostream>

using namespace std;

int main(void) {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);

  int n;
  size_t k;
  cin >> n >> k;

  auto ls = Vec(n);
  for (size_t i = 0; i < n; ++i) {
    cin >> ls[i];
  }

  auto answer = solution(std::move(ls), k);

  cout << answer << "\n";

  return 0;
}
```
