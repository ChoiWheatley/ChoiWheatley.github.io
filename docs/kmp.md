---
description:
title: kmp
created: 2023-02-13T06:16:30
categories: 
 - 
aliases: 
 - KMP
tags: [" algo/kmp algo/string ", algo/kmp, algo/string]
date created: Monday, February 13th 2023, 6:16:30 am
date modified: Monday, February 27th 2023, 6:20:45 pm
updated: 2024-02-07T20:56:48
---
parent link: [[0011 Algorithms ♾️|algorithms]]

---

# kmp

상태: 풀이완료  
태그: string

[boj.kr/16916](http://boj.kr/16916) 

# TL;DR

- 문자열 매칭은 찾기를 수행하려는 텍스트 (T)와 찾으려는 문자열인 패턴 (P)가 존재, N = T.length, M = P.length라고 하자.
- O(NM)을 O(N + M)으로 만들어주는 알고리즘이 바로 KMP 알고리즘이다.
- P를 T에 하나씩 갖다 대면서 찾기를 수행할 때, 비록 T의 \[i, i + m)번째 문자들과 P의 \[0, m)번째 문자들은 일치하지만 m+1 번째 문자가 일치하지 않았더라도 KMP는 P를 다시 0번째로 되돌아가지 않게 만들어준다. 바로 지금까지 일치했던 문자들을 활용해서 말이다.
- P의 접두어와 m까지의 접미어가 서로 일치하면 T를 읽을 때 일치했던 문자들을 다시 볼 필요가 없다.

# 예시 (백준 1786번)

```
      0 1 2 3 4 5 6 7 8 9 …
T : [ A B C D A B C D A B D E ]
      | | | | | | X
P : [ A B C D A B D ]
      0 1 2 3 4 5 6
```

T의 [0, 5] 문자와 P의 [0, 5] 문자는 동일하지만 6번째 인덱스에서 해당 찾기는 실패하고 만다. 하지만 그렇다고 T를 1번째 인덱스부터 다시 읽자니 비효율적이다. 이 경우 우리는 P에서 규칙을 찾아낼 수 있다.

P의 [0, 1] 번째 문자와 [4,5] 번째 문자는 서로 중복이다. 그리고 6번째 문자에서 틀렸으므로 T의 [0, 1]번째 문자와 [4,5]번째 문자 또한 모두 똑같다. 이 경우 T[4]에서부터 시작하고 처음 두 글자는 무시하고 계속 검색을 이어나가면 된다.

```
      0 1 2 3 4 5 6 7 8 9 …
T : [ A B C D A B C D A B D E ]
              | | | | | | |
P :         [ A B C D A B D ]
              0 1 2 3 4 5 6
```

# But.. How?

P를 분석하여 인덱스별 접두사와 접미사의 길이를 리턴하는 fail 함수를 만든다. 위의 예시에서 P에 대한 fail 함수는 다음과 같다.

| x | P[0:x] | fail(x) |
| --- | --- | --- |
| 0 | A | 0 |
| 1 | AB | 0 |
| 2 | ABC | 0 |
| 3 | ABCD | 0 |
| 4 | ABCDA | 1 |
| 5 | ABCDAB | 2 |
| 6 | ABCDABD | 0 |

T가 0번째 인덱스에서 검색을 진행중인 상황으로 돌아가자. i = 0,  j = 6, (i는 T의 인덱스, j는 P의 인덱스)일 때, 우리는 j를 0으로 돌려버릴 것이 아니라 fail(j - 1) = 2로 만들 수 있다. i는? i + (j - 2) = 4부터 시작하면 되겠지. 그러면 우리는 T[4:10] == P[0:6]임을 알게 되고 검색결과 중 하나를 노출할 수 있게 된다!

# 다른 예시 들고와

[https://m.blog.naver.com/kks227/220917078260](https://m.blog.naver.com/kks227/220917078260) 에서 가져옴

```
      0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5
T : [ A B A A B A A A B A A B A A B A]
      | | | | | | | X
P : [ A B A A B A A B A ]
      0 1 2 3 4 5 6 7 8
```

이번엔 특별한 예시를 들고왔다. 바로 P의 접두사와 접미사의 길이가 P 길이의 절반을 넘는 경우다. 이 경우 P에 대한 fail 함수는 다음과 같이 그려질 수 있다.

| x | P[0:x] | fail(x) |
| --- | --- | --- |
| 0 | A | 0 |
| 1 | AB | 0 |
| 2 | ABA | 1 |
| 3 | ABAA | 1 |
| 4 | ABAAB | 2 |
| 5 | ABAABA | 3 |
| 6 | ABAABAA | 4 |
| 7 | ABAABAAB | 5 |
| 8 | ABAABAABA | 6 |

6번째부터 접미사와 접두사가 P 길이의 절반을 넘기 시작해 서로의 영역을 침범하기 시작했다. 이래도 되는걸까? 당연히 된다. 따라서 T[i: i + 8]일 때 검색에 실패했다면, i를 fail(7) = 5 만큼만 빼면 될 것이다. j는? 당연히 5부터 다시 시작하면 된다. 위의 예시를 다시 들고오자.

```
      0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5
T : [ A B A A B A A A B A A B A A B A]
            | | | | X
P :       [ A B A A B A A B A ]
            0 1 2 3 4 5 6 7 8
```

i = 0, j = 7일 때 실패하였으므로, 다음 `i := i + (j - fail(j - 1))` = 7 - 4 = 3부터 세면 된다. j := fail(j - 1) = 4부터 검사하면 된다. 아쉽게도 검사하자마자 실패했다. 

이번에도 마찬가지로 `i := i + (j - fail(j - 1))` = 3 + (4 - 1) = 6으로 이동하고, `j := fail(j - 1)` = 1로 움직이자.

```
      0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5
T : [ A B A A B A A A B A A B A A B A]
                  | X
P :             [ A B A A B A A B A ]
                  0 1 2 3 4 5 6 7 8
```

이번에도 마찬가지로 `i := i + (j - fail(j - 1))` = 6 + (1 - 0) = 7, `j := fail(j - 1)` = 0으로 움직이다.

```
      0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5
T : [ A B A A B A A A B A A B A A B A]
                    | | | | | | | | |
P :               [ A B A A B A A B A ]
                    0 1 2 3 4 5 6 7 8
```

마참내! i = 7일 때 j = 0..8까지 모두 일치하여 검색을 성공하게 되었다.

i는 절대로 감소하지 않는다. 무슨 말이냐면, P가 중간에 뒤로 돌아가지 않는다는 말이다. 하지만 방금 사례에서와 같이 접두사와 접미사의 길이가 P의 길이의 절반을 넘어가는 경우, 한 번에 점프하지 않고 여러 차례에 나눠서 그리고 점진적으로 조금씩 이동하는 모습을 볼 수 있었다. 이는 향후 구현코드에서 글자 불일치에 대한 조건문을 통해 더 자세히 알 수 있을 것이다.

# pseudo code - matching

```python
def solution(T: String, P: String):
  fail: int[] = makeFail(P)
	j = 0
	for i in range(len(T)) :
		while (j > 0 and T[i] != P[i]):
			# 글자 불일치
			j = fail[j - 1]
		if (T[i] == P[j]):
			# 글자 일치
			if (j == len(P)-1):
				# 성공!
				when_match_with_index(i - j)
				j = fail[j - 1]
			else: 
				j += 1
```

코드는 마찬가지로 저 네이버 블로그 포스팅을 참고했다. i를 사용하는 방식이 살짝 달라졌는데, 다음과 같다.

old i : 비교를 시작하는 T의 인덱스

new i: 당장 비교하려는 T의 인덱스, `new_i == old_i + j`

j : 비교하려는 P의 인덱스 (= 일치하는 부분 문자열의 길이)

따라서 11번째 줄 `when_match_with_index` 안에 들어있는 인덱스 `i - j` 가 이해가 될 것이다. 패턴 일치시 우리는 T에서 패턴이 시작하는 인덱스를 원하기 때문에 이 작업을 수행한 것이다.

4번째 줄의 while문에 주목하라. 통상적인 if문이 아니라 while문을 통해 계속해서 j를 fail 함수에 넣는 이유는 바로 위에서 설명했던 P의 접두사와 접미사의 길이가 P의 길이의 절반을 넘은 상황에 대응하기 위해서이다. “ABAABAABA”와 같은 경우에서 우리는 T의 비교위치(7)에 머물러서 몇 차례에 걸쳐 j의 값을 수정했다. 보다시피 비교하고자 하는 위치 7은 그대로이기 때문에 i == 7일 때 일치하는 P[j]를 찾거나 그 어느것도 일치하지 않을 때까지 j를 fail 함수에 넣어야 한다.

# making fail function

사실 매칭과정에서 O(N+M)이 들었다 하더라도 fail 함수를 만드는데 O(M^2)의 시간복잡도가 들면 아무짝에 쓸모도 없다는 것은 자명한 사실이다. 따라서 우리는 P와 P를 비교하면서 fail 함수를 만들어야 할 것이다.

인덱스가 0인 경우는 0임에는 자명하다. 따라서 이 경우는 건너뛴다. 

| x | P[0:x] | fail(x) |
| --- | --- | --- |
| 0 | A | 0 |

x는 i + j이다.

i = 1인 경우 j = 0이자마자 실패하기 때문에 0으로 처리한다.

```
      0 1 2 3 4 5 6 7 8
P : [ A B A A B A A B A ]
        X
P':   [ A B A A B A A B A ]
        0 1 2 3 4 5 6 7 8
```

| x | P[0:x] | fail(x) |
| --- | --- | --- |
| 0 | - | 0 |
| 1 | AB | 0 |

i = 2인 경우 j = 0은 성공하였으므로, 접두사와 접미사의 공통부분의 길이가 1이 된다. 하지만 j = 1일 때 실패하였으므로 다음 j는 fail(j-1) = 0으로 변경한다.

```
      0 1 2 3 4 5 6 7 8
P : [ A B A A B A A B A ]
          | X
P':     [ A B A A B A A B A ]
          0 1 2 3 4 5 6 7 8
```

| x | P[0:x] | fail(x) |
| --- | --- | --- |
| 0 | A | 0 |
| 1 | AB | 0 |
| 2 | ABA | 1 |

i = 3인 경우 j = 0..5 모두 일치한다. 따라서 fail함수를 i+j = 3..8까지 모두 작성할 수 있다.

```
      0 1 2 3 4 5 6 7 8
P : [ A B A A B A A B A ]
            | | | | | |
P':       [ A B A A B A A B A ]
            0 1 2 3 4 5 6 7 8
```

| x | P[0:x] | fail(x) |
| --- | --- | --- |
| 0 | - | 0 |
| 1 | AB | 0 |
| 2 | ABA | 1 |
| 3 | ABAA | 1 |
| 4 | ABAAB | 2 |
| 5 | ABAABA | 3 |
| 6 | ABAABAA | 4 |
| 7 | ABAABAAB | 5 |
| 8 | ABAABAABA | 6 |

# pseudo code - making fail function

```python
def createFail(P: String):
	failArr = [0] * len(P)
	j = 0
	for x in range(1, len(P)):
		while (j > 0 and P[x] != P[j]):
			# 글자 불일치
			j = failArr[j - 1]
		if (P[x] == P[j]):
			# 글자 일치
			j += 1
			failArr[x] = j
	return failArr
```

매칭 알고리즘과 모습이 거의 비슷하다. 다만 글자 일치시 failArr의 값에 중복길이를 집어넣는다는 것이 차이점이 되겠다.

인덱스 0을 의도적으로 생략하는 부분도 확인바란다. 만약 저 for 문을 0부터 돈다면, 모든 인덱스에서 글자가 일치하는 대참사가 발생한다.

---

[KMP 알고리즘(Knuth-Morris-Pratt Algorithm) (수정: 2019-09-01)](https://m.blog.naver.com/kks227/220917078260)


___

# 관련 문제

- [1786번: 찾기](https://www.acmicpc.net/problem/1786)
- [4354번: 문자열 제곱](https://www.acmicpc.net/problem/4354)
- [2401번: 최대 문자열 붙여넣기](https://www.acmicpc.net/problem/2401)
- [2902번: KMP는 왜 KMP일까](https://www.acmicpc.net/problem/2902)
- [9253번: 스페셜 저지](https://www.acmicpc.net/problem/9253)
- [1305번: 광고](https://www.acmicpc.net/problem/1305)
- [11585번: 속타는 저녁 메뉴](https://www.acmicpc.net/problem/11585)
- [1893번: 시저 암호](https://www.acmicpc.net/problem/1893)
- [7575번: 바이러스](https://www.acmicpc.net/problem/7575)
- [boj-1701: Cubeditor](https://boj.kr/1701)
