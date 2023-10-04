---
aliases: 
tags: 
description:
title: 이전기수 QNA 정리 {swjungle}{pintos}{project2}
created: 2023-10-04T15:45:02
updated: 2023-10-04T18:33:22
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
 
- `intr_frame`과 `plm4` 테이블은 커널 영역에 있다. plm4는 유저 가상 주소를 물리 주소로 매핑하는 자료를 가지고 있다. (page table과 동일한건가?)
	- 그래서 USER_PROGRAM이 실행되면 발생되는 일이... User virtual address를 가리키는 plm4 테이블을 찾아 그 위치로 rsp의 값을 변경한다. 그 뒤에 rip 값을 유저 프로그램 첫 위치로 옮긴 뒤 usermode로 실행.
 
- 프로그램에서 처음 실행되는 함수는 리턴 주소가 없어서 fake address를 넣는다.
- 커널이 호출하는 `printf`는 `lib/kernel/console.c:vprintf`를 참조한다. 반면에 유저 프로그램이 호출하는 `printf`는 `lib/user/console.c:vprintf`를 참조한다. 이 차이를 알아야 `write` 시스템 콜을 만들 수 있다.
- `pid_t`와 `tid_t`와의 차이점: `tid_t`는 프로세스 안에서 식별할 수 있는 스레드 식별자이고 `pid_t`는 프로세스와 커널에서 식별할 수 있는 프로세스 식별자이다. 
