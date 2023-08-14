---
aliases: 
tags: 
description:
title: 도수정렬 (Counting Sort) {rust} {python}
created: 2023-08-14T16:47:35
updated: 2023-08-14T18:46:06
---
- [[0011 Algorithms ♾️]]
- [수 정렬하기 3 {boj}](https://www.acmicpc.net/problem/10989)
- [Rust 풀이](https://www.acmicpc.net/source/58913744)
- [Python 풀이]  
사실, [[Doit 자료구조와 함께 배우는 알고리즘 기초 파이썬 편]]에서 공부했을 때에 나온 도수정렬이 무엇이었는지 감도 제대로 안왔는데, 4월중에 내가 한창 러스트에 빠져있었을 때 도수정렬을 사용하여 문제를 풀었던 적이 있어 깜짝 놀라고 말았다.

1. 도수분포표를 만든다. 어차피 원소의 개수는 정해져 있으니까 그 최댓값으로 초기화 한다.
2. 0이 아닌 모든 원소 `el`과 그 인덱스 `i`에 대하여:
	1. `el`은 곧 그 원소가 나타난 횟수이므로, 그 횟수만큼 출력한다.
	2. PROFIT 💰

## rust source code

```rust
use std::io;
use std::io::Write;

const MAX_V: usize = 10_000;
const CAPACITY: usize = 4096000;
type ElemType = u32;

fn main() -> Result<(), io::Error> {
    let mut lines = io::stdin()
        .lines()
        .flat_map(|res_str| res_str.map(|s| s.parse::<usize>().expect("parse error")));
    let _n = lines.next().unwrap();

    let mut num_list = [0 as ElemType; MAX_V + 1];

    while let Some(num) = lines.next() {
        num_list[num] += 1;
    }

    let mut output = io::BufWriter::with_capacity(CAPACITY, io::stdout());

    for (i, el) in num_list.into_iter().enumerate().filter(|(_, el)| *el > 0) {
        for _ in 0..el {
            write!(output, "{}", format!("{}\n", i))?;
        }
    }

    Ok(())
}
```

## python source code

```python
"""도수정렬을 사용한 문제풀이
참고: https://www.acmicpc.net/source/58913744"""
from sys import stdin, stdout

MAX_V = 10_000
num_list = [0 for _ in range(MAX_V + 1)]
n = int(input())

for line in stdin:
    num_list[int(line.strip())] += 1

for idx, cnt in enumerate(num_list):
    if cnt == 0:
        continue
    for _ in range(cnt):
        stdout.write(str(idx) + "\n")
```
