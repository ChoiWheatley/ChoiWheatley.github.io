---
aliases: 
tags: 
description:
title: week08 - User Program {pintos} {swjungle}
created: 2023-10-15T04:42:56
updated: 2023-10-15T04:43216:59
---

### INDEX

- csapp
	- [[3. Machine Level Representation of Programs {CSAPP}]]
	- [[8. Exceptional Control Flow]]  | 8.2, 8.3, 8.4
- faq
	- [pintos-kaist/project2/FAQ.html](https://casys-kaist.github.io/pintos-kaist/project2/FAQ.html)
	- [지난 기수의 질문 아카이브](https://jungle7-7610626261f4.herokuapp.com/pages/pintos-questions2.html)
	- [[각종 QNA 정리 {swjungle}{pintos}{project2}]]
- pintos-kaist
	- [gitbook :: Project 2 Introduction](https://casys-kaist.github.io/pintos-kaist/project2/introduction.html)
	- [gitbook :: Appendix :: Virtual Address](https://casys-kaist.github.io/pintos-kaist/appendix/virtual_address.html)
	- [gitbook 한글번역 {Notion}](https://www.notion.so/PROJECT-2-USER-PROGRAM-b019874b02f645d7813c554bd7377770)

### 2023-10-03 발제 아카이브

- gpt, bing chat, bard, 클로바 x 등등 가리지 말고 다 써봐라. 생성형 AI가 언제나 정답을 이야기해주지는 않으니.
- TDD spec이라고 불리우는 엔티티는 specification의 약자로, "이렇게 이렇게 돌아갔으면 좋겠어요" 요구사항을 명세로 작성할 때부터 이미 테스트케이스로 만들어버린다. 그리고 테스트 코드를 통과하도록 코드를 작성한다.
	- TDD가 좋은 점은 컴퓨터에게 단순 노가다를 맡길 수 있다는 점이다. TDD를 맹신하면 안되는게, 테스트 코드 통과에만 목을 매다는 경우가 발생할 수 있기 때문이다.
- week07 Thread을 구현하면서 느낀점 - Timer HW가 없으면 스레드도 없다. ⟶ preemptive scheduling
- [x] [[Monitor의 condvar가 하나일 필요가 없나?]]
- PRJ2 질문목록을 옆에 끼고, GPT와 여러 생성형 AI를 옆에 끼고 살자. 내가 할 질문은 이미 누군가가 했을수도 있다.
- [[3. Machine Level Representation of Programs {CSAPP}]]에서 나오는 레지스터들과 기타 하드웨어 알아둘 지식들 [[Hardware Knowledges for PintOS {swjungle}]]
- [[8. Exceptional Control Flow]]
	- 8.2. Processes
	- 8.3. System Call Error Handling
	- 8.4. Process Control
- Argument Passing
	- 기본적인 function call에서조차 단순 `call` 과 `ret` 만으로 함수 호출 / 반환이 이루어지지 않는다. 반드시 레지스터의 도움을 필요로 한다.
	- 실행파일이 인자와 함께 메모리에 올라가는 과정에 대한 이해가 필요할 것이다.
- Process
	- Abstraction of Machine
	- Protection --- HW의 도움을 받아야 해.
	- system call --- 총 14개나 되는데, 그걸 다 구현해야 해. 근데 그게 다가 아니야. arg passing, user mem access를 다루고 난 뒤에야 진행할 수 있음
	- Process가 끝날 때엔 무슨 일이 일어나지?
	- fd 복제 with [[dup2(2)]]

![[Pasted image 20231003145914.png]]

### 2023-10-04

- [[각종 QNA 정리 {swjungle}{pintos}{project2}]]
- limitation from simple file system
	- 파일 시스템 코드를 사용하는 동안 internal synchronization을 사용하지 마세요. Project4가 진행되기 전까지는 한 번에 하나의 프로세스가 파일 시스템 코드를 실행한다는 것을 보장하는 용도로만 synchronization을 사용하세요.

- **argument passing**
	- `process_exec()` 함수는 새로운 프로세스에 인자를 전달하는 것을 지원하지 않음. 따라서 이를 확장구현하여 공백을 기준으로 여러 단어로 나누어지게 만들어라.

### 2023-10-05

- how to run?
	- to run and grade a single test, `make` the `'.result'` file explicitly from the `build` directory.

	 ```shell
	make tests/threads/alarm-multiple.result 
	```

### 2023-10-06

- [[argument passing flow {pintos}]]

- 자주 보이는 주솟값들 모음
	- `KERN_BASE`: 0x8004000000 | 549822922752 | 가상메모리 상에서 커널 주솟값의 시작주소
	- `USER_STACK`: 0x47480000 | 1195900928 | 가상메모리 상에서 유저 프로세스 스택영역의 출발지점

- syscall_handler에는 syscall 번호와 인자가 들어간다.

	> Thus, when the system call handler `syscall_handler()` gets control, the system call number is in the **rax**, and arguments are passed with the order **%rdi, %rsi, %rdx, %r10, %r8, and %r9**.

- system calls + user memory access
	- @세준: file based system calls
	- @승현 + @가람: user memory access, process related system calls

- `page_fault()`
	- cr3: page directory base register.
	- cr2: page fault linear address ⟶ 어디에서 fault가 발생했는지를 저장하는 `fault_addr` 변수에 저장  
	- `exception_init()` 안에서 해당 함수를 인터럽트 핸들러로 등록한다.  
`

### Weekly I learned

[[Project2 User Program {wil}]]
