---
aliases: 
tags: 
description:
title: Multi Level Feedback Queue {swjungle}{pintos}
created: 2023-09-30T15:05:14
updated: 2023-10-01T14:44:25
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

Nice는 직역하자면 "친절한"이란 뜻이다. Nice 값이 높은 스레드는 다른 스레드에게 자신의 CPU 자원을 "친절하게" 퍼다준다. Nice의 값은 -20부터 20까지의 값을 가지며, 음수일 경우 반대로 다른 스레드의 CPU 시간을 빼앗는다. 스케줄러는 Nice 값에 기반한 공식을 사용하여 여러 스레드들의 우선순위를 동적으로 매겨 전체적인 response time을 줄이는 데 힘쓴다.

구현하여야 하는 코드는 [`threads/thread.c`](https://github.com/ChoiWheatley/swjungle-week07-09/blob/master/threads/thread.c) 파일 안에 있다.

- `thread_get_nice`
- `thread_set_nice`

### Calculating Priority

$$
\text{priority}=\text{PRI\_MAX}-\frac{\text{recent\_cpu}}{4}-\text{nice} \times 2
$$

`recent_cpu`는 해당 스레드가 점유한 시간을 의미한다. 따라서 한 스레드가 오랜 시간 CPU를 점유할수록 우선순위는 떨어지게 된다. nice 값이 높으면 재선점될 일이 자주 찾아오지 않을 것이다. priority 값은 매 4 틱마다 계산을 수행하게 된다.

### Calculating `recent_cpu`

스레드가 지금까지 잡아먹은 시간을 리턴한다. 단순히 선형적으로 증가하는 것이 아니라 decay를 고려하여 점점 증가속도가 감소하도록 만들었다.

```c
recent_cpu = (2 * load_avg)/(2 * load_avg + 1) * recent_cpu + nice
```

`load_avg`는 대기하는 스레드의 개수의 평균을 의미한다.

### Calculating `load_avg`

지수적으로 가중치가 쌓이는 형태의 평균 대기 스레드 수를 리턴한다.

```c
load_avg = (59/60) * load_avg + (1/60) * read_threads
```

### Fixed-Point Real Arithmetic

17.14 고정소수점 실수연산을 사용해야 한다. 여기에서 32비트 정수형을 기준으로 LSB부터 14개의 비트는 fractional bits, 즉 소수부를 의미하고 17개의 비트는 decimal points, 정수부를 의미한다. 나머지 1 비트는 당연하게도 부호비트.

## Tests

- load-1: 하나의 스레드를 45초동안 재운다(busy waiting). 그 뒤에 `load_avg`를 검사하는데, 0.5가 나오게 만들어주어야 한다. 그 다음으로 10초동안 재운다(block). 그 뒤에 `load_avg`를 검사하는데, 0.5보다 작게 나오게 만들어야 주어야 한다.
- load-60: 60개의 스레드를 시작하여 동시에 10초동안 sleep 한다. 그리고 60초동안 busy wait를 하고 다시 60초동안 sleep한다. 세 과정 모두 60개의 스레드가 동시에 참여한다. main 스레드는 매 2초마다 전체 `load_avg`를 검사하는데, 처음 60초 까지는 계속 증가하다가 그 이후부터 감소해야 한다.
- load-avg: 60개의 스레드를 시작하는데 i번째 스레드는 `10 + i`초를 잔 뒤에 60초동안 busy waiting을 한다. 그리고 main 시작을 기점으로 120초가 지난 시점까지 다시 sleep한다. main 스레드는 매 2초마다 전체 `load_avg`를 검사한다.
- recent-1
- fair-2
- fair-20
- nice-2
- nice-10
- block
