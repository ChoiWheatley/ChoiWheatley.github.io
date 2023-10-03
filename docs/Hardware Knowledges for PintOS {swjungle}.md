---
aliases: 
tags: 
description:
title: Hardware Knowledges for PintOS {swjungle}
created: 2023-10-03T14:27:58
updated: 2023-10-03T14:32:06
---
- [[week07-09 {swjungle} {pintos}]]
___

## README

본 문서는 pintos 주차때 필수적으로 대답할 수 있어야 할 HW, SW 관련 지식에 대한 코치님이 출제한 질문목록을 그대로 가져왔습니다. [[3. Machine Level Representation of Programs {CSAPP}]]를 읽어야 대답할 수 있는 문제가 많으니 참고하면서 하나씩 답해봅시다.

- %rip, %rsp register의 의미와 CPU 동작에서의 역할
- assembly 명령어 JMP와 CALL의 차이점
- JMP, CALL, RET, PUSH, POP, IRET 명령에서 %rip, %rsp는 어떻게 값이 변하는가?
- interrupt 발생 시 CPU는 어떻게 동작하는가? register 값 및 연관된 memory는?
- interrupt를 처리한 후에는 어떻게 되는가?
- interrupt는 어떻게 enable/disable 시킬 수 있는가? enable/disable 되면 뭐가 달라지는가?
- Option: NMI (Non-Maskable Interrupt)는 무엇인가?
- timer interrupt는 누가 발생시키는가?
- EFLAGS register는 무엇인가?
- Option: "플래그 세우지 마라"의 그 flag인가?
- Option: 왜 다른 64bit register처럼 r로 시작하지 않고 e로 시작하는가?
