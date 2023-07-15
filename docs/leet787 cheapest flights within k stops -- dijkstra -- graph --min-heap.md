---
description:
aliases: 
tags: 
created: 2023-05-26T17:25:57
updated: 2023-07-15T21:33:04
title: leet787 cheapest flights within k stops -- dijkstra -- graph --min-heap
---
- 테스트케이스가 책이 출판된 뒤에 변경이 되어 타임아웃이 발생해서 멘붕이 왔었다.
- https://leetcode.com/problems/cheapest-flights-within-k-stops/
- notice: 책에 있는 코드로는 더 이상 ACCEPT가 나오지 않는다. 다음 링크를 참조하여 더 효율적인 코드에 대한 토론을 확인해보자.  
    https://github.com/onlybooks/algorithm-interview/issues/104

# Commit Logs

1. [DEAD END](https://github.com/OrmiCodeRanger/ChoiSeunghyeon/blob/e09b612d77e88350a3d216ef709e71a0f435b237/leet787.py)  
	다익스트라 함수를 변경하지 않고 기능을 추가할 생각에 들떴으나, depth를 구할 방도를 찾지 못해 책을 펼쳤다.
 
2. [야, 이거 갱신이 안 되는데?](https://github.com/OrmiCodeRanger/ChoiSeunghyeon/blob/9be66b4d2960f10f3d695e2699857998a5b0a3b1/leet787.py)  
	그래서 위에서 `predicate`를 제거하고 다익스트라 함수의 재사용성을 포기하더라도 `max_depth`를 추가하였으나... `dist`갱신이 이루어지지 않아 테스트 케이스에서 실패했다. 

	좀 더 구체적으로, `cur.weight <= dist[cur.vertex]` 에 의해 더 작은 weight가 갱신되기는 하다. 하지만 정작 더 작은 depth에 의해 갱신되지는 않는다는거.

	근데 솔직히 이 코드를 조금만 손 보면 가능할 것 같기도 한데? 

3. [다익스트라에 집착하면 안 될 것 같다.](https://github.com/OrmiCodeRanger/ChoiSeunghyeon/blob/e22beb123d5420c9ac5780c3d69b5855764c2c1a/leet787.py)  
	`dist`를 `dict`에서 `list`로 바꾸고 위키에서 나온것처럼 무한대 값을 주어서 다시 풀었지만 뭐 달라진 건 없었다. 이런 필요없는 실험을 줄여야 시간을 아낄 수 있다.

	그래서 다익스트라 함수를 제거했다. 이때부터 교재를 보면서 문제를 풀기 시작함. 내가 만든 테스트 케이스들이 모두 통과하고 나는 이대로 끝날 줄 알았다...

4. [Time Limit Exceeded 💀](https://github.com/OrmiCodeRanger/ChoiSeunghyeon/blob/647d42f4f44f96d355f910d17e3391c2f16cf060/leet787.py)  
	교재의 코드를 보면서, 좀 더 의미론적인 방식대로 버무려 코딩했다. (같은 알고리즘을) 이때 내가 의심한 부분이 있기는 하네. 

	> 2순위: min weight -- 왜 아저씨 코드에는 minimum 계산이 없지?

	교재의 코드의 단점은 바로 `dist`를 아예 빼버렸다는 것이다. 따라서 꺼낸 노드가 이미 계산이 끝난 노드면 더 이상 방문하지 않지만 이 풀이는 그부분을 해제해 버렸기 때문에 중복해서 탐색하는 부분이 생기게 된다. (설마, 무한루프까지?)

5. [ACCEPTED](https://github.com/OrmiCodeRanger/ChoiSeunghyeon/blob/589ae40c6adbef0b6049727133784923d8e93610/leet787.py)  
	기존 코드를 뒤집어엎고 다른 분의 코드를 기반으로 재작성했다. 중복탐색을 줄이면서 동시에 문제조건이었던 **within K-stops** 를 체크하는 조건문은 말로 쓰자면 다음과 같다.

	1. `dist`: 박상길코드의 그 `dist`와 성격이 다르다. 얘는 모든 버텍스의 `weight`와 `depth`를 저장한다. 
	2. `queue` 안에 이웃 노드를 추가하는 조건문이 변경됐다.
		1. 이전에는 각 노드가 갖고 있던 `k`(남은 depth)만 비교하고 최소비용 판별은 `heapq`가 알아서 하게 만들었다. 
		2. 현재 코드는 weight의 합이 더 유리한지, ==OR== depth가 더 유리한지 판별 후 push한다.
