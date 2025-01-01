---
links:
status:
aliases: 
tags:
description:
title: 1005 ACM Craft
created: 2023-08-23T00:03:37
updated: 2025-01-01T22:15:07
---

# 1005 ACM Craft

태그: graph, topological_sorting  
URL: boj.kr/1005  
상태: 풀이완료  
최종 편집 일시: 2023년 4월 20일 오후 6:57

[백준 1005] C++ ACM Craft(위상정렬)](<<https://hwan-shell.tistory.com/313>)>

inpired by this blog

# TL;DR

- 입력으로부터 연결리스트를 만든다.
- 위상정렬을 수행한다.
- 위상정렬 결과를 순회하며 weight를 연결된 노드들로 전파한다.
    - 이때 동시에 여러 브랜치에서 도달하는 동일한 노드들 간에 전파된 weight를 비교해야 할 수 있는데, 문제의 조건에 따라 더 큰 것을 전파하면 된다.

# 풀이 1. DFS

[choi-workspace/solution.hpp at 877b24296d903a550e2f35cd7146dc62b59858d5 · ChoiWheatley/choi-workspace](https://github.com/ChoiWheatley/choi-workspace/blob/877b24296d903a550e2f35cd7146dc62b59858d5/ALGORITHMS/bak/problem/1005/solution.hpp)

처음엔 단순 DFS로 풀었다. 당연히 시간초과가 발생했지만. DFS로 처음 접근한 것 자체는 나쁘지 않았다. 중복 노드를 여러번 반복 접근하기 때문에 O(N * N)의 시간복잡도가 나왔고 에지의 개수가 최대 10 ** 6이나 되기 때문에 제곱이 아닌, 상수시간으로 문제를 풀어야 했다는 것을 인지한 상태에서 문제를 풀기 시작했다.

괜히 swea에서 배웠던 캐시 효율이 좋다고 하는 [flatten 1D linked list](https://www.notion.so/graph-29f71a5029ec4983b6adb834e17c137a?pvs=21)를 구현하려고 시도하는 바람에 시간이 배로 오래 걸렸다. 확실히 똑같은 int형들을 하나는 인덱스로 쓰고, 하나는 포인터로 쓰고, 하나는 값으로 쓰려니까 실수의 여지가 너무 많다. 

어쨌든 `w` 노드서부터 재귀적으로 노드를 방문하면서 말단노드까지 — 즉, 나는 방향성 그래프의 방향을 반대로 세워놓은 채로 문제를 풀었다. — 방문하면서 발생할 수 있는 모든 경우에 대한 최대 weight의 합을 리턴하도록 만들었다. 정답은 대충 맞는 것 같았지만 시간초과가 나와 힌트를 볼 수 밖에 없었다.

# 풀이 2. 위상정렬

[choi-workspace/solution.hpp at 6331da9eecacfdcb3250d51b8427baef8654ea39 · ChoiWheatley/choi-workspace](https://github.com/ChoiWheatley/choi-workspace/blob/6331da9eecacfdcb3250d51b8427baef8654ea39/ALGORITHMS/bak/problem/1005/solution.hpp)

swea 특강에서 온라인 수업에 잠깐 배운 기억이 있지만 문제를 풀어본 것은 이번이 처음이다. 따라서 사실상 까맣게 잊고 있었던 위상정렬을 인터넷에 검색해가며 공부했다. 자세한 구현방법은 [[위상정렬]] 을 참고하라.

나는 욕심이 나서 아까 구현해두었던 flatten 1D linked list를 그대로 사용하면서 위상정렬을 구현하려고 했고, 이번에도 시간을 펑펑 낭비하고 말았다. 💸💸💸💸 지나간 건 지나갔다. 정리를 마저 끝내자. 

풀이1번에서 내가 direction의 방향을 뒤집어 놓았기 때문에 원래 방향대로 돌려놓을 필요가 있었다. 이제 1 → 2→ 3 관계에 대하여 3을 수행하기 위해 2가 필요하고 2를 수행하기 위해 1이 필요한 그래프가 갖추어졌다. 

1. 위상정렬 with recursion
    
    `visited` 비트셋과 결과 스택을 저장할 `out` 가변 레퍼런스를 인자로 넣어 DFS처럼 호출을 시도한다. 이때 DFS와 다른 점은 바로 `push`의 타이밍에 있다. DFS는 탐색한 노드를 먼저 `push`한 뒤에 재귀함수를 호출한다. 하지만 위상정렬은 의존성이 높은 — 그러니까 A → B 그래프에서 B의 의존성이 A보다 높다 — 원소를 먼저 스택에 `push`해야한다. 따라서 탐색 및 재귀호출이 모두 끝난 뒤에 시행한다.

    ```cpp
    struct Node {
    	int id;
    	int next;
    };
    
    array<Node, K + 1> nodes;
    array<int, N + 1> head;
    
    void topological_sort(int head_id, BitSet &visited, vector<int> &out) {
    	auto node_ptr = head[head_id];
    	while (node_ptr != -1) {
    		// 탐색 및 재귀
    		auto node = nodes[node_ptr]; 
    		if (!visited[node.id]) {
    			visited.set(node.id);
    			topological_sort(node.id, visited, out);
    		}
    		node_ptr = node.next;
    	}
    	// 스택에 푸시
    	visited.set(head_id);
    	out.push_back(head_id);
    }
    ```

2. call `topological_sort`
    
    위상정렬을 수행하는 코드는 알고보니 반복문에 넣어야만 했다. 다만, 이미 방문한 노드들은 굳이 방문할 필요가 없기 때문에 관련 체크를 같이 수행해주면 된다.

    ```cpp
    // for i in 0..N
    if (!visited[i]) {
    	topological_sort(i, visited, sorted_rev);
    }
    ```

3. propagate weights
    
    내가 구현한 위상정렬은 스택이니까, 가장 마지막에 있는 원소가 처음 수행해야만 하는 작업이고 앞으로 갈수록 의존도가 높은 작업들이다. 그래서 걍 `for_each` 로 거꾸로 순회하도록 만들어버렸다. 
    
    그래서 각각의 노드들에 대하여 위상정렬 할 때처럼 인접노드들을 순회하며 `result` 벡터 — `weights` 벡터의 클론 — 의 원소들에 현재 노드가 가지고 있는 `weight`만큼을 더해주어야 한다. 하지만 다음과 같은 상황이 발생할 수 있다.

    ```mermaid
    graph LR
      1 --> 2 --> 4
    	1 --> 3 --> 4
    ```

    위는 문제에서 제공하는 예시와 같은데, 각각의 weight도 동일하게 1번부터 [10, 100, 1, 10] 이라고 치자. 위의 코드대로 위상정렬을 수행하면 아마 아래와 같을 것이다.

| i | 1 | 2 | 3 | 4 |  
| --- | --- | --- | --- | --- |  
| sorted_rev[i] | 4 | 3 | 2 | 1 |  

1. `sorted_rev.pop()` ⇒ 1
	
	1과 연결된 노드들에 `weight[1] = 10`을 더한다.

| i | 1 | 2 | 3 | 4 |  
| --- | --- | --- | --- | --- |  
| weight | 10 | 100 | 1 | 10 |  
| result | 10 | 110 | 11 | 10 |  

1. `sorted_rev.pop()` ⇒ 2
	
	2와 연결된 노드들에 `weight[2] = 110`을 더한다.

| i | 1 | 2 | 3 | 4 |  
| --- | --- | --- | --- | --- |  
| weight | 10 | 100 | 1 | 10 |  
| result | 120 | 110 | 11 | 10 |  

1. `sorted_rev.pop()` ⇒ 3
	
	3과 연결된 노드들에 `weight[3] = 11`을 더한다.

| i | 1 | 2 | 3 | 4 |  
| --- | --- | --- | --- | --- |  
| weight | 10 | 100 | 1 | 10 |  
| result | 120 | 110 | 11 | 10 |

변함이 없다. 왜냐하면 `result[1]`이 이미 더 큰 값인 120을 가지고 있기 때문이다. 갱신하지 않고 다음으로 넘어간다.
	
1. `sorted_rev.pop()` ⇒ 4
	
	4와 연결된 노드는 없고 심지어 4는 W이므로 그대로 리턴한다.
