---
aliases: 
tags: 
description:
title: 13334 철로 {boj} {priorityqueue}
created: 2023-08-24T09:30:03
updated: 2023-08-24T09:50:10
---
[문제풀이링크 {PR}](https://github.com/ChoiWheatley/swjungle-week-02/blob/e3a4687ea2758e85016c4b0d4342ac7653e54219/ghdud4653/26_21606(%EC%95%84%EC%B9%A8%EC%82%B0%EC%B1%85).py)

끝점을 기준으로 정렬한다. 순회를 하면서 우선순위 큐에 집어넣는데, 이때 최대한 많은 사람들의 집과 사무실을 포함하기 위해서 시작점을 기준으로 minheap에 넣는다. 이렇게 하면 좋은 점이, heap에는 끝점의 위치와 D만큼의 길이를 보장하는 구간만이 존재하게 되기 때문이다.
