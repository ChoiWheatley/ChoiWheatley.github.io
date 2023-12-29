---
aliases: 
tags: 
description:
title: 각종 QNA 정리 {swjungle}{pintos}{project2}
created: 2023-10-04T15:45:02
updated: 2023-10-08T17:20:56
---
- [[week07-10 {swjungle} {pintos}]]
- [pintos-kaist/project2/FAQ.html](https://casys-kaist.github.io/pintos-kaist/project2/FAQ.html)
- [지난 기수의 질문 아카이브](https://jungle7-7610626261f4.herokuapp.com/pages/pintos-questions2.html)
___
- 권유집 교수님의 자료는 original pintos를 기준으로 진행. 즉 32비트 운영체제를 기준으로 진행한다. 또 각종 메서드-like 함수들의 이름이 바뀌었으니 이 점 참고하면서 다루어야 한다.
- 함수 인자는 최대 6개까지만 레지스터 `rdi, rsi, rdx, rcx, r8, r9`에 저장되고 그보다 많은 인자는 스택에 넣어 전달된다.

- **syscall** 호출시 일어나는 과정

	> user mode에서 돌아가던 테스트케이스 프로그램이 `syscall` 이라는 명령어를 실행하면 현재 실행되는 프로세스는 일시중지되며 하드웨어가 커널에게 신호를 보내서 커널의 /userprog/syscall-entry.S 에 적힌 syscall_entry가 시작됩니다. 명령어가 순차적으로 진행되며 몇 가지 백업과 준비를 마친 후, `moveabs $syscall_handler, %r12; call *%r12` 라는 명령어를 통해 /userprog/syscall.c 의 syscall_handler 가 실행되는 겁니다. syscall_handler의 실행이 종료되면 다시 syscall-entry.S에 돌아와서 나머지 명령어를 실행하고 sysretq를 통해 테스트케이스 프로그램으로 돌아갑니다.

	- `userprog/syscall-entry.S:syscall_entry` 플래그로 문맥이 이어간다는 점. `userprog/syscall.c:syscall_init`의 주석을 확인하자.

		> 시스템 호출. 이전에 시스템 호출 서비스는 인터럽트 핸들러에 의해 처리되었습니다 (예: 리눅스에서의 int 0x80). 그러나 x86-64에서는 제조업체가 시스템 호출을 요청하기 위한 효율적인 경로인 'syscall' 명령을 제공합니다. 'syscall' 명령은 Model Specific Register (MSR)에서 값을 읽는 방식으로 작동합니다. 자세한 내용은 매뉴얼을 참조하십시오

	- syscall handler는 인터럽트를 끄지 않는다.
	- 

- **어셈블리**
	- `$Imm(%reg)`: `%reg` 에 저장되어있는 주소에 `$Imm`만큼 더한 위치
	- [?] `moveabs`
	- `$tss`는 `userprog/tss.c` 안에 정의된 `struct task_state *tss`라는 전역변수가 저장된 메모리 주소를 가져옵니다. 

		> Task-State Segment (TSS)는 x86 아키텍쳐의 문맥교환에 사용됩니다. 하지만 x86-64에서 문맥교환(context switching = task switching)은 지원이 중단된 기능입니다. 그래도 TSS는 여전히 ring switching 동안 스택 포인터를 찾아내기 위해 사용되고 있습니다.

	- [rsi, rdi registers {SO}](https://stackoverflow.com/questions/23367624/intel-64-rsi-and-rdi-registers): source index, destination index reg 들의 용법
		- `movsb`는 `DS:SI` (DataSegment:SourceIndex) 부터 `ES:DI` (ExtraSegment:DestinationIndex) 사이의 바이트를 복사하기 위한 연산자이다. 그런데 calling convention에 따라야만 하는 건 아니라서... 꼭 저 이름대로 쓸 필요는 없대.
	- `push`: call을 하기 전 callee 스택에 인자들을 등록하는 과정. immediate operand, memory location을 push 할 수 있다. push가 일어날 때마다 rsp가 해당 인자의 크기만큼 줄어든다. 반대로 스택으로부터 값을 회수하는 명령어는 `pop`이다.

 - **plm4 and intr_frame** 
	- `intr_frame`과 `plm4` 테이블은 커널 영역에 있다. plm4는 유저 가상 주소를 물리 주소로 매핑하는 자료를 가지고 있다. (page table과 동일한건가?)
		- 그래서 USER_PROGRAM이 실행되면 발생되는 일이... User virtual address를 가리키는 plm4 테이블을 찾아 그 위치로 rsp의 값을 변경한다. 그 뒤에 rip 값을 유저 프로그램 첫 위치로 옮긴 뒤 usermode로 실행.
	- [plm4, page map level 4의 유래](https://www.pagetable.com/?p=14)
	- [[plm4 virtual address coverage formula {pintos}]]
 
- 유저 프로그램에서 처음 실행되는 함수는 리턴 주소가 없어서 fake address를 넣는다.
- 커널이 호출하는 `printf`는 `lib/kernel/console.c:vprintf`를 참조한다. 반면에 유저 프로그램이 호출하는 `printf`는 `lib/user/console.c:vprintf`를 참조한다. 이 차이를 알아야 `write` 시스템 콜을 만들 수 있다.
- `pid_t`와 `tid_t`와의 차이점: `tid_t`는 커널 스레드 식별자이고 커널에서만 사용될 수 있다. `pid_t`는 사용자 프로세스를 식별한다. `exec`, `wait` 시스템콜에서 이를 사용한다. 둘 다 int형이고 tid, pid 관계를 일대일 매핑으로 만들거나 복잡한 매핑을 만들 수 있다.
- `e_entry`는 elf 바이너리 파일에서 응용 프로그램이 처음 시작되어야 하는 명령어의 위치가 담겨있다.
- rox: Read Only for eXecutable
- `process_init`: 프로세스 처음 만들 때 초기화하는 함수. 초기에는 `initd`에서만 호출하지만 필요에 따라서 프로세스를 만들거나 복제할 때 호출해도 된다.

- **fork**
	- `tf` : fork를 수행하던 커널이 어디까지 작업했는지에 대한 정보가 저장된다. `threads/thread.c:thread_launch()` 참고.
	- `f`: 시스템 콜 핸들러가 `process_fork`에게 인자로 넘기는 부모의 user-level 정보가 담긴 `struct intr_frame f`
	- 
	- ![[IMG_2858.jpg]]
		- 부모 프로세스가 fork를 호출하던 당시의 컨텍스트를 `f`, `process_fork` 안에서 `thread_create`를 호출할 당시의 컨텍스트를 `tf`라고 부른다. 이때 `tf`를 자식 프로세스에게 줄 것이 아니라 `f`를 전달해야 올바르게 `fork`를 호출했던 시점의 PC부터 시작할 것이다. 이 정보는 `syscall_handler(f)`의 인자로 전달되고 `thread::bf` 백업 인터럽트 프레임 멤버에 저장되어 자식 프로세스 생성을 위한 첫 스레드 `__do_fork`에서 복제된다 [`process.c:__do_fork`](https://github.com/ChoiWheatley/swjungle-week07-09/blob/5ac563a178c6314a7a41104aa15e280981dff40c/userprog/process.c#L175), [`syscall.c:syscall_handler`](https://github.com/ChoiWheatley/swjungle-week07-09/blob/5ac563a178c6314a7a41104aa15e280981dff40c/userprog/syscall.c#L100-L103)
		- user-level information에는 레지스터 정보 (rax, rbx, rcx, rip, etc)가 들어있고 스택 포인터가 들어있고 Code Segment Selector (CS), Stack Segment Selector(SS), User Flags, User Data Segment Selectors(DS, ES, FS, GS), User stack 등이 저장된다.
		- *CS* : 유저 프로그램의 Code Segment를 결정하는데 사용된다. 허가되지 않은 메모리 영역 내에 액세스 하지 못하도록 하는 데 사용된다.
		- *SS*: CS와 마찬가지로 스텍 세그먼트를 결정하는 데 사용된다.
		- *DS, ES, FS, GS*: 유저 프로그램의 Data Segment들을 결정하는데 사용된다. 각각 DataSegment, ExtraSegment(요즘 안쓰는 string operation을 위한 세그먼트), Function specific Segment(프로세스 안 스레드(Thread Local Storage)를 위해서), General purpose Segment

- *eflag*
	- extended flag의 약자로, 프로그램 실행에 있어서 필요한 다양한 상태를 저장하고 있는 레지스터이다. 프로그래머는 에러 핸들링, 대수연산, 분기 따위를 다룰때 사용된다. 대표적으로 zero flag(ZF), carry flag(CF), overflow flag(OF), **interrupt flag(IF)**, sign flag(SF), parity flag(PF), direction flag(DF), trap flag(TF), alighment check(AC), resume flag(RF)가 있다.

- *argument passing*
	- **Calling convention에 따르면 %rdi는 함수의 첫 번째 인자, %rsi는 함수의 두 번째 인자에 해당합니다.** argument passing을 하고 나서 유저 프로그램이 처음 실행되면 프로그램의 entry point에 해당하는 함수가 가장 먼저 실행되는데, 여기서 main함수를 호출합니다. main함수는 `int main(int argc, char **argv)` 와 같은 식으로 선언하다시피, **argument passing에서 %rdi와 %rsi에 저장한 argc와 argv의 값이 main함수로 전달되어 일반 프로그래머가 프로그램을 실행하는 데 사용된 parameter를 분석할 때 사용할 수 있습니다.**

- *pintos program*
	- 명령어를 하나씩 실행시키고 싶다면 `make tests/threads/alarm-multiple.result`와 같은식으로 .result파일을 직접 입력해주면 됩니다. 더 자세한 정보는 [https://web.stanford.edu/class/cs140/projects/pintos/pintos.pdf](https://web.stanford.edu/class/cs140/projects/pintos/pintos.pdf) 여기에 가셔서 5페이지를 참고하시면 될 것 같습니다.
