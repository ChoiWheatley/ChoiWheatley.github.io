---
aliases: 
tags: 
description:
title: buses and hierarchical buses {os}
created: 2023-08-30T16:43:48
updated: 2023-08-30T16:50:47
---
- [[0015 OS {ssu2021-2nd} 💻|OS]]
- 05주차 내용 p.3 참고

# singular bus => bottleneck

버스란, CPU, RAM, IO장치 간 데이터가 이동하는 통로. 단일버스로 구현됐을 당시엔 CPU와 부가기기의 속도 차이가 크지 않았음. 그러나 CPU의 속도가 압도적으로 빨라지며 IO의 응답을 마냥 기다리가민 하는 상황이 발생.

# hierarchical buses

cpu local bus, memory bus, PCI bus, etc.

빠른 CPU, Memory는 별도의 시스템 버스에 연결하고 IO장치는 IO 버스에 연결하여 IO작업이 진행되는 동안 CPU가 기다리지 말고 다른 일을 할 수 있게끔
