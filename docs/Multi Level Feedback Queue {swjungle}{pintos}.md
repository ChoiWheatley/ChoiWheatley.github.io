---
aliases: 
tags: 
description:
title: Multi Level Feedback Queue {swjungle}{pintos}
created: 2023-09-30T15:05:14
updated: 2023-09-30T17:04:18
---
- [[week07-09 {swjungle} {pintos}]]
- [pintos-kaist/Advanced Scheduler](https://casys-kaist.github.io/pintos-kaist/project1/advanced_scheduler.html)
___

## README

[[week07-09 {swjungle} {pintos}]] Project 1의 마지막 과제! NICE가 무엇인지, aging 기법을 활용했을 때 어떻게 starvation을 막을 수 있는지 여부를 알아낸 뒤 내가 어떤 것을 구현하여야 하는건지 목표를 파악한 뒤에 빠르게 구현까지 끝내보자.

## Summary

> 4.4 BSD Scheduler와 같은 multi-level-feedback-queue 스케줄러를 구현하여 평균 응답시간을 줄여라.

pintos 명령줄에 `-mlfqs` 옵션을 넣으면 `threads/thread.h`에 선언된 `thread_mlfqs` 전역변수가 TRUE로 수정된다. 이 경우 thread_create를 할 때 지정한 인자 중 우선순위 값을 무시한다. 더 이상 명시적으로 우선순위 지정이 불가하며, `thread_set_priority`, `thread_get_priority`는 스케줄러가 부여한 우선순위를 리턴하여야 한다. ~~아니, setter도?~~

mlfqs는 우선순위 기부를 하지 않기 때문에 우선순위 기부를 제외한 코드를 따로 호출하도록 만들어야 한다. 

### 4.4BSD Scheduler

범용목적 스케줄러는 존재한다! IO 작업이 많은 스레드는 응답시간이 빨라야 하지만 CPU time은 적게 먹는다는 특징이 있다. 컴퓨트 자원을 많이 요하는 스레드는 응답시간보다는 CPU time을 더 많이 먹는다. 대부분의 스레드는 응답시간, CPU time 요구량이 시간에 따라 변화한다. 범용목적 스케줄러는 이 모두를 만족시키기 위해 존재한다.

우리는 [McKusick] 방식의 스케줄러를 도입할건데, 자세한 내용은 Appendix에 있으니 참고바란다. 어쨌든 이 mlfqs는 말 그대로 여러 큐를 관리하며 그 중에서 가장 높은 우선순위를 가진 비어있지 않은 큐에 들어있는 스레드를 먼저 실행한다. 스케줄러가 큐 하나를 선택했다면 그 안에 있는 스레드들을 Round Robin 순서로 실행하게 된다.

> Multiple facets of the scheduler require data to be updated after a certain number of timer ticks. In every case, these updates should occur before any ordinary kernel thread has a chance to run, so that there is no chance that a kernel thread could see a newly increased `timer_ticks()` value but old scheduler data values.  
> 스케줄러의 여러 측면에서는 특정 수의 타이머 틱 후에 데이터를 업데이트해야 합니다. 모든 경우에 이러한 업데이트는 일반 커널 스레드가 실행되기 전에 이루어져야 커널 스레드가 새로 증가된 timer_ticks() 값을 볼 수 있지만 이전 스케줄러 데이터 값을 볼 수 있는 가능성이 없습니다.

...?

### Niceness

### Calculatiing Priority

### Calculating `recent_cpu`

### Calculating `load_avg`

### Summary

### Fixed-Point Real Arithmetic

