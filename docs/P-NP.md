---
aliases: 
tags: 
description:
title: P-NP
created: 2024-02-07T16:27:43
updated: 2024-02-07T18:39:12
---
- <https://gazelle-and-cs.tistory.com/64>
---

P = deterministic Polynomial, NP = Nondeterministic Polynomial

결정론적(deterministic) 문제들은 문제의 답이 오직 Yes / No로 나오는 문제를 의미한다. 비결정론적(nondeterministic) 문제들은 문제의 답이 

[P vs NP 쉽게 이해하기](https://gazelle-and-cs.tistory.com/64) 블로그에 따르면 어떤 질문에 Yes / No로 답할 수 있거나 배열을 정렬하는 것과 같이 같은 입력으로 같은 출력이 나오는 문제를 결정론적이라고 말할 수 있습니다. 반면에 날씨를 예측한다거나 외판원이 모든 도시를 방문하는 가장 저렴한 경로를 찾는 문제와 같이 문제를 해결하기 위한 경로 혹은 그 결과가 매번 달라질 수 있는 문제를 비결정론적이라고 말할 수 있습니다.

cubeditor 문제는 적어도 두 번 중복되는 부분문자열 중 가장 긴 문자열을 찾는 문제입니다. 정답의 길이는 1일 수도 있고 2, 3, ... 또는 심지어 정답이 아예 없을 수도 있습니다. 따라서 수많은 경로를 지니는 문제이므로 비결정적 문제라고 말할 수 있습니다. "\~~의 값을 최대화 하세요" 같은 비결정적 문제를 때론 최적화 문제라고도 부릅니다. 

그러면 cubeditor 문제의 한 입력으로 abc (문제의 문자열의 부분문자열이라 합시다)가 주어졌을때 이 문제의 답이 True 혹은 False임을 답하는데 얼마나 오래 걸리나요? 슬라이딩 윈도우를 사용하여 N번만 훑으면 되므로 다항시간 안에 풀리기 때문에 cubeditor는 Nondeterministic Polynomial, 줄여서 NP라고 부릅니다. (아님)

Cubeditor 문제는 비결정론적인 문제가 아닌 것으로 보임.
