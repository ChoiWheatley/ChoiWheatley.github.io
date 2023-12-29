---
aliases: 
tags: 
description:
title: 2023-10-10 권영진 교수님 OS 강의 (2차) {swjungle}
created: 2023-10-10T10:30:01
updated: 2023-10-10T12:48:00
---
level of indirection에 대해서 배웠다! addr translation mech가 대표적임. 여러 translation mech가 존재하는데 fixed partition, segmentation, 등등 여러 구현방법은 3-easy-pieces 안에 들어있다. 

파일시스템을 안한다고 해서 중요한 포인트만 짚고가보자. 커널이 load할 때 하는게 뭐예요, 커널 영역 할당하고 스택영역 초기화(stack setup)하고. 사실 OS도 load되어야 하는 프로그램이기 때문에, 최초의 부팅에는 로더에 의해 돌아간다.

여유가 있다면 핀토스의 COW가 있다. 카이스트 학생이 가장 싫어한 주제. demand paging을 사용하기 때문에 코드 구조를 저시기 해보면 많은 도움이 될 것이다.

## 다시 file system, level of indirection부터...

mmap의 옵션 중 MAP_ANONYMOUS는 메모리 영역을 힙에 매핑하겠다는 의미. fd는 무시된다. virtual address는 physical address는 임의의 함수로 매핑한다고 했다. 이것 중 우리는 페이징 기법을 사용하고, 이것이 level of indirection의 한 형태.

file system또한 비슷하게 구현된다. 다만 physical 메모리가 아닌 file과 storage block 을 offset을 통해 매핑을 한다. 디자인 출발점은 비슷하나, 특이한 차이점이 존재한다.

1. 영속성
2. 느린 속도

memory 관기는 cpu 안에 mmu 하드웨어가 존재하여 page 테이블만 os가 만들어 놓으면 알아서 되는데, file system은 위와 같은 특이점으로 인해 다양한 level of indirection이 존재.

## abstraction of storage

path + filename으로 식별, offset으로 위치. 이렇게 매핑해주는 엔티티를 파일 시스템이라고 부르자. 인덱싱을 하는 방법은 여러가지고 대표적으로 inode가 있다. 

인덱스 정보는 영속성을 위해 스토리지에 들어가 있다. 

언제 physical block에 올리죠? 메모리는 page fault가 발생할때 (in access time)라면 vi 편집기에서는 `:w`를 할 때! 이게 시스템 콜로 변환. 그게 `write` 시스템 콜이냐? `fsync`가 좀 더 명확함. 얘가 발생하면 파일시스템이 block들을 alloc한다. 

batching: 유저버퍼에 모아뒀다가 write 시스템 콜이 호출되면 커널버퍼로 메모리를 복사한다. fsync 시스템 콜이 호출되면 이 모든 내용을 스토리지 디바이스에 넣는다. 반대도 마찬가지. **이 디자인이 성능에 악영향을 미친다.**

DRAM의 메타데이터와 실제 데이터가 같이 올라와있다. length가 9 ⟶ 10으로 바뀌었다고 하자. 얘를 disk로 올리고자 하는 경우, 이 두개의 데이터를 잘 싱크해야 하는 책임이 발생한다. 하지만 HW는 한 번에 한 블럭만 옮길 수 있다. 그래서 application developer들에게 이 책임을 떠넘긴다. 4KB 기준으로 원자적으로 적힌다. 두 개의 블럭이 동시에 옮기는 일은 file system이 보장하지 않는다. 

**crash consistency**

글자 하나가 1블럭이라고 가정할때 Foo를 Bar로 바꾸고 싶어. atomicity는 제대로 바뀌었거나 아예 바뀌기 전이거나 둘 중 하나의 결과만을 보장해야 한다. 그 책임이 누구에게 있다고? user program이! 

느리더라도 백업(수정본을 or 이전버전을)을 저장해놓는다. 각각 roll back log과 write ahead log로 불리운다.

```c
creat(/a/log)
write(/a/log, "Foo") // 1
write(/a/file, "Bar") // 2
unlink(a/log)
```

하지만 이것으로는 부족하다. 1, 2의 순서를 보장하기 위해 fsync를 넣어야 한다.

하지만 이것으로도 부족하다. 로그파일을 못찾을 수도 있다. 따라서 directory에게 이것이 작성되었다는 소식을 전달해야 한다. 따라서 디렉토리에게 fsync를 해야한다.

```c
creat(/a/log)
write() // 1
fsync(/a/log)
fsync(/a/)
write() // 2
fsync(/a/file)
unlink
```

OS는 유저에게 위험한 기능을 제공하고 프로그래머가 직접 프로토콜을 지키도록 강요하게 만든 이유는 뭔가요? ⟶ 성능을 위해서 유저에게 선택지를 준 것이다. 물론 ext journeling 모드라고 모든 read, write 사이사이 로깅을 해주는 녀석이 있다.

## protection

app이 커널을 훼손시키지 못하도록 / app이 다른 app을 훼손시키지 못하도록.

- `hlt` 인스트럭션은 코어를 idle 상태로 만든다. 유저 프로그램이 이 인스트럭션을 사용하도록 허용하면 OS가 픽픽 꺼질것이다. 그래서 중요한 인스트럭션만 커널이 쓸 수 있게 만들어보자. ⟶ privileged instruction
- 유저 app이 다른 메모리 공간을 읽거나 쓰지 못하게 만들어야 한다. ⟶ memory isolation
- 무한루프로 인해 다른 시스템이 CPU time을 확보하지 못하는 일이 없어야 한다. ⟶ timer interrupt

### privilege separation

HW endorses privilege to OS. ring3개를 사용하는 x86 시스템. ring 0 = 모든 instruction을 포함, ring 3 = 특정 inst를 배제. 어떻게 ring 번호를 바꿔달 수 있는지 여부는 알아서 찾아보셈

user application이 요청한 주소를 page table에 보고 mmu가 인터럽트를 발생시키는 구조.

segmentation fault ≠ page fault

- DPL (Desciptor Privilege Level)이 0번이면 kern, 3이면 user. 
- page table entry는 4 KB 페이지를 매핑하는데, 오프셋은 12비트로 매핑한다. 나머지 20비트는 pte에 있음. 그러면 12비트가 비네? 그 비트를 가지고 여러 플래그를 설정한다.
	- supervisor / user bit
	- dirty bit ⟶ fsync때 활용

### isolation

**memory isolation**

다른 프로세스에 있는 데이터를 읽거나 쓰지 못한다. + 다른 프로세스의 인스트럭션으로 jmp하지 못한다. data confinement + control flow confinment | **classes** in OOP

instance A, instance B는 서로 분리가 되어있다. language 상으로는 class가 되고, OS 상으로는 virtual address이고 HW를 이용한 page table이다.

**file isolation**

직접 file을 접근하는 인스트럭션은 privileged instruction. 커널이 실제로 중간에 임의의 시점에서 읽어도 되는 파일인지 여부를 판단하는 reference monitor라고 부른다. file perm system resides in.

해당 디자인을 구현하기 위한 level of indirections

- DAC (Discretion Access Control): rwx owner이걸 결정하는 뭐시기.
- MAC (Mandatory Access Control)

IPC를 통해서 정해진 api(protocol)을 통해서만 소통할 수 있게 만들어놨다. shared memory, message passing

- shared memory: 두 개의 프로세스가 같은 메모리를 매핑할 수 있도록 허용. Physical memory? no. CPU Cache를 산지직송한다. (cache coherence)
	- signal 방식 vs polling 방식으로 동기화를 한다.
- pipe: producer는 kernel에 열심히 쓰고, consumer는 kernel에서 열심히 먹는다. 하지만 전송해야 하는 양이 많을수록 느려진다. (three-easy-pieces 교재에서 확인바람.)

### Sharing HW resources

cpu time sharing (scheduling) & memory space sharing (swapping)

> mechanism과 policy를 분리하라.

- Policy: what should be done? ⟶ 결정이다.
- Mechanism: how to do something? ⟶ 수단이다.

mechanism(scheduler)이 분리되어있으면 round robin, mlfqs 등등 여러 policy들을 갈아끼울 수 있게된다. 예를 들어 `next_to_run`이 policy를 구현한 함수.

**time sharing with scheduling**

지표: turnaround time & response time

preemptive Round Robin ≠ non-preemptive Round Robin [[01_os_review.pdf {swjungle}#^g34tuq]]

**space sharing with swapping**

anon mem에 대해서 page fault가 발생하면 0으로 채운 physical memory에 free frame을 채운다. 언젠가 물리메모리가 꽉찼다. 이 경우 victim을 결정해야 한다. (mechanism)

page replacement algo (policy)

지표: swapped count

미래를 알 수 없기 때문에 temporal locality를 활용해야 한다. 가장 오랫동안 안 읽힌거를 evict 시킨다. (LRU) loop가 많은 프로그램은 temporal locality가 증가. 다만, 유튜브 같은 놈을 LRU로 돌리면 바보됩니다.

LRU는 커널이 구현 못한다. 언제 어디 메모리가 읽힐지 예측할 수 없다. HTTP request는 object가 결정이 되기 때문에 가능하지만 메모리를 읽는데 api를 보내지는 않잖아. lru appoximation을 하기 위해 clock을 사용한다.

## How to design OS

- monolithic kern: linux, windows, ...
- microkernel: OS 서브시스템을 유저레벨 프로그램으로 올려서 신뢰도를 높인다. 대신 되게 느리다.
- [x] exokernel의 단적인 예시로 임베디드 프로그램을 들 수 있을까? 
	- No. amazon lambda의 요청된 앱 하나에 최적화된 컨테이너
- unikernel: 운영체제를 라이브러리화하여 프로세스로 올려놓음.
