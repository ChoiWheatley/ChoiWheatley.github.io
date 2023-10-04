---
aliases: 
tags: 
description:
title: 각종 QNA 정리 {swjungle}{pintos}{project2}
created: 2023-10-04T15:45:02
updated: 2023-10-04T20:06:21
---
- 권유집 교수님의 자료는 original pintos를 기준으로 진행. 즉 32비트 운영체제를 기준으로 진행한다. 또 각종 메서드-like 함수들의 이름이 바뀌었으니 이 점 참고하면서 다루어야 한다.
- 함수 인자는 최대 6개까지만 레지스터 `rdi, rsi, rdx, rcx, r8, r9`에 저장되고 그보다 많은 인자는 스택에 넣어 전달된다.

- **syscall** 호출시 일어나는 과정

	> user mode에서 돌아가던 테스트케이스 프로그램이 `syscall` 이라는 명령어를 실행하면 현재 실행되는 프로세스는 일시중지되며 하드웨어가 커널에게 신호를 보내서 커널의 /userprog/syscall-entry.S 에 적힌 syscall_entry가 시작됩니다. 명령어가 순차적으로 진행되며 몇 가지 백업과 준비를 마친 후, `moveabs $syscall_handler, %r12; call *%r12` 라는 명령어를 통해 /userprog/syscall.c 의 syscall_handler 가 실행되는 겁니다. syscall_handler의 실행이 종료되면 다시 syscall-entry.S에 돌아와서 나머지 명령어를 실행하고 sysretq를 통해 테스트케이스 프로그램으로 돌아갑니다.

	- `userprog/syscall-entry.S:syscall_entry` 플래그로 문맥이 이어간다는 점. `userprog/syscall.c:syscall_init`의 주석을 확인하자.

		> 시스템 호출. 이전에 시스템 호출 서비스는 인터럽트 핸들러에 의해 처리되었습니다 (예: 리눅스에서의 int 0x80). 그러나 x86-64에서는 제조업체가 시스템 호출을 요청하기 위한 효율적인 경로인 'syscall' 명령을 제공합니다. 'syscall' 명령은 Model Specific Register (MSR)에서 값을 읽는 방식으로 작동합니다. 자세한 내용은 매뉴얼을 참조하십시오

- **어셈블리**
	- `$Imm(%reg)`: `%reg` 에 저장되어있는 주소에 `$Imm`만큼 더한 위치
	- [?] `moveabs`
	- `$tss`는 `userprog/tss.c` 안에 정의된 `struct task_state *tss`라는 전역변수가 저장된 메모리 주소를 가져옵니다.
	- [rsi, rdi registers {SO}](https://stackoverflow.com/questions/23367624/intel-64-rsi-and-rdi-registers): source index, destination index reg 들의 용법
		- `movsb`는 `DS:SI` (DataSegment:SourceIndex) 부터 `ES:DI` (ExtraSegment:DestinationIndex) 사이의 바이트를 복사하기 위한 연산자이다. 그런데 calling convention에 따라야만 하는 건 아니라서... 꼭 저 이름대로 쓸 필요는 없대.
 
- `intr_frame`과 `plm4` 테이블은 커널 영역에 있다. plm4는 유저 가상 주소를 물리 주소로 매핑하는 자료를 가지고 있다. (page table과 동일한건가?)
	- 그래서 USER_PROGRAM이 실행되면 발생되는 일이... User virtual address를 가리키는 plm4 테이블을 찾아 그 위치로 rsp의 값을 변경한다. 그 뒤에 rip 값을 유저 프로그램 첫 위치로 옮긴 뒤 usermode로 실행.
 
- 유저 프로그램에서 처음 실행되는 함수는 리턴 주소가 없어서 fake address를 넣는다.
- 커널이 호출하는 `printf`는 `lib/kernel/console.c:vprintf`를 참조한다. 반면에 유저 프로그램이 호출하는 `printf`는 `lib/user/console.c:vprintf`를 참조한다. 이 차이를 알아야 `write` 시스템 콜을 만들 수 있다.
- `pid_t`와 `tid_t`와의 차이점: `tid_t`는 커널 스레드 식별자이고 커널에서만 사용될 수 있다. `pid_t`는 사용자 프로세스를 식별한다. `exec`, `wait` 시스템콜에서 이를 사용한다. 둘 다 int형이고 tid, pid 관계를 일대일 매핑으로 만들거나 복잡한 매핑을 만들 수 있다.
- `e_entry`는 elf 바이너리 파일에서 응용 프로그램이 처음 시작되어야 하는 명령어의 위치가 담겨있다.
- rox: Read Only for eXecutable
- `process_init`: 프로세스 처음 만들 때 초기화하는 함수. 초기에는 `initd`에서만 호출하지만 필요에 따라서 프로세스를 만들거나 복제할 때 호출해도 된다.

- **fork**
	- `tf` : fork를 수행하던 커널이 어디까지 작업했는지에 대한 정보가 저장된다. `threads/thread.c:thread_launch()` 참고.
	- `f`: 시스템 콜 핸들러가 `process_fork`에게 인자로 넘기는 부모의 user-level 정보가 담긴 `struct intr_frame f`
	- ![[IMG_2858.jpg]]
		- user-level information에는 레지스터 정보 (rax, rbx, rcx, rip, etc)가 들어있고 스택 포인터가 들어있고 Code Segment Selector (CS), Stack Segment Selector(SS), User Flags, User Data Segment Selectors(DS, ES, FS, GS), User stack 등이 저장된다.
		- *CS* : 유저 프로그램의 Code Segment를 결정하는데 사용된다. 허가되지 않은 메모리 영역 내에 액세스 하지 못하도록 하는 데 사용된다.
		- *SS*: CS와 마찬가지로 스텍 세그먼트를 결정하는 데 사용된다.
		- *DS, ES, FS, GS*: 유저 프로그램의 Data Segment들을 결정하는데 사용된다. 각각 DataSegment, ExtraSegment(요즘 안쓰는 string operation을 위한 세그먼트), Function specific Segment(프로세스 안 스레드(Thread Local Storage)를 위해서), General purpose Segment
