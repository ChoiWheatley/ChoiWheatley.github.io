---
aliases: 
tags: 
description:
title: week07 WIL 정리, 발표준비 {swjungle}
created: 2023-10-02T19:12:43
updated: 2023-10-02T19:53:12
---
- [[week07-09 {swjungle} {pintos}]]
___

## README

아래는 코치님의 WIL 작성요령에 관한 안내사항이다.

- WIL은 weekly I learned 의 약자입니다. (TIL = Today I learned 와 비슷)
- OS Project 진행하시면서 정리하실 기회를 드립니다.
- 미리 작성하셔도 좋고 틈틈이 TIL을 작성하셨으면 그 TIL들을 종합하거나 링크만 남겨두셔도 좋습니다.
- WIL은 공유해 주신 블로그 혹은 notion에 정리해 주시면 됩니다.
- Slack에 적어 놓은 질문은 나중에 지워지므로 질문 답변 내용을 정리해 두는 것도 나중에 복습할 때 도움 될 수 있습니다.
- WIL 작성은 팀 별 발표와는 별도의 개인 별 과제입니다. 그렇지만, 팀 별 발표에서 한 팀원의 WIL의 일부 혹은 전부를 자료로 활용할 수 있습니다.

## brainstorming

- Debugging tools, git 협업하면서 얻은 지식들: 도구 사용방법을 배웠다고 그걸 발표하는게 좋으려나.. 방법론 이야기 꺼내기 좋은 자리는 아닌 것 같기도 하다
	- `debug_panic`에 breakpoint를 달아놓아 커널패닉시 backtrace 하기 유리하게
- `priority-donate-sema`
	- ready list는 정렬해서 넣는데 waiters(lock, cond)도 정렬해서 넣었더니 중간에 들어오는 donation에 의해 순서가 뒤바뀌는 경우 발생. ⟶ waiters에 넣을때 push back, lock release 또는 cond signal 할 때 max pop
- `priority-donate-chain`
	- lock acquire 시점이 아니라 get_priority시점에 연쇄적으로 기부받은 우선순위를 재귀적으로 찾아들어간다.
