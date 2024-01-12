---
aliases: 
tags: 
description:
title: C++ remainder operation is NOT modulo
created: 2024-01-11T16:59:21
updated: 2024-01-11T16:59:40
---
- [[C++]]
---
[Mod of negative number is melting my brain](https://stackoverflow.com/questions/1082917/mod-of-negative-number-is-melting-my-brain/20924698#20924698)

음수를 처리할 필요가 있는 mod는 `%` 연산을 통해 원하는 결과를 내보내지 않는다. 예를 들어 `-1 mod 3` 은 2와 동치이다. 왜냐면 $-1 = 2 + k \times 3$을 만족시키는 k = -1을 가지고 있기 때문이다. 하지만 `-1 % 3` 은 -1을 리턴하는데, 단순히 -1을 3으로 나눈 나머지를 1을 3으로 나눈 나머지에 음수를 씌운 -1이라고 주장하기 때문이다.

`%` 연산은 언어에 따라 결과가 다르기도 하다. 디버깅을 수행하며 파이썬으로 계산했을 땐 `-1 % 3 == 2`라고 나오는 걸 보면 파이썬의 `%` 연산은 모듈로 연산인 것이다.

어쨌든 나머지 연산과 모듈로 연산이 이런 부분에서 극명한 차이가 있다는 것을 깨달았으니 모듈로 연산을 직접 정의해서 사용할 필요가 있어졌다.

1. `x mod m` 에서 x, m 모두 양수임이 확실한 경우,
    
    `x mod m == x % m`
    
2. m > 0일 경우, x는 음수일 수도 있을때,

    ```cpp
    int mod(int x, int m) {
    	int r = x % m;
    	return (r < 0 ? r + m : r);
    }
    ```

3. m도 음수일 가능성이 있을때, (가장 안전하겠죠?)

    ```cpp
    int mod(int x, int m) {
    	int r = x % m;
    	if ((m > 0 && r < 0) ||
    			(m < 0 && r > 0)) {
    		return r + m;
    	}
    	return r;
    }
    ```

# 모듈로와 관련한 문제

[algo](https://www.notion.so/algo-26c08b5f469647c4b138435c32501b4f?pvs=21)

[14565번: 역원(Inverse) 구하기](https://www.acmicpc.net/problem/14565)

모듈로 역원

[23062번: 백남이의 여행 준비의 준비](https://www.acmicpc.net/problem/23062)

중국인의 나머지 정리
