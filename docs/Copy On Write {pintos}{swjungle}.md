---
aliases: 
tags: 
description:
title: Copy On Write {pintos}{swjungle}
created: 2023-10-22T05:50:32
updated: 2023-10-22T19:10:22
---
- [[week09 - Virtual Memory {pintos} {swjungle}]]
- [[11. memory mapping and Copy-on-write (COW) {SP}]]
___

## wiki 정리

<https://en.wikipedia.org/wiki/Copy-on-write>

PTE ⟶ mark as read-only, keep count of the **number** of the refs

- only one ref ⟶ skip new allocation
- greater than 1 ⟶ allocate new page, decrementing ref count from original page

[[Drawing 2023-10-22 06.00.07.excalidraw]]

![[Pasted image 20231022105535.png]]

## cow-simple은 통과하는데...

hidden test case가 존재하지만 정확히 어떤 부분을 컨트롤해야 하는지 모르겠다. 지금 커널모드의 WP(Write Protection) 비트도 꺼져있어 반쪽짜리 COW임이 분명하고 권 교수님 말씀대로 구조를 뜯어고친것도 아닌데 테스트케이스가 통과하는 것이 매우 의심스럽다. 숨겨진 테스트 케이스를 알아내기 위해서 질문방에 글을 올렸으나, 조교진들도 알 수 없다는 답변에 좀 더 구체적인 힌트를 요구하기 위한 질문을 작성하려고 한다.

다음 시나리오들은 저희가 생각해본 cow 메커니즘이 올바르게 작동하는지 여부를 확인하기 위한 테스트들입니다. 추가할만한 채점 고려사항이나 테스트 자체의 의도에 대해서 첨언해주신다면 정말 감사하겠습니다.

- 먼저 swap in/out을 적극적으로 사용하는 시나리오입니다. 부모가 페이지 크기의 지역변수를 선언합니다. 그 뒤로 자식 프로세스를 여러개 선언한 뒤 각 프로세스가 지역변수를 write하게 만들어 COW와 swap이 동시에 의도한 대로 이루어지는지를 테스트합니다.
	- 테스트케이스는 유저프로그램의 입장에서 실행되는건데, 유저 프로그램이 특정 페이지가 swap in되었는지 out되었는지 알기는 힘들 것 같습니다. 그것은 커널이 관리하는 영역이지, 유저 프로그램한테 노출하는 정보는 아니기 때문입니다.
- syscall을 통해서 간접적으로 cow를 테스트할 수도 있을 것 같습니다. 저희 코드는 현재 CR0 레지스터의 write protect bit를 꺼놓은 상태로 구현을 했습니다. 이 경우 유저 프로세스가 직접 write 요청을 통한 페이지 복제는 가능하지만 `read`, `write`를 통한 간접적인 버퍼 쓰기요청에는 page fault가 발생하지 않을 것으로 예상됩니다.
	- 네 syscall을 이용한 테스트케이스는 필요해 보입니다. 말씀해주신 상황에서 커널이 값을 쓰는 경우에도 page fault가 발생하여 copy-on-write를 해야합니다.
- 저희는 현재 프레임의 reference count가 1인 프레임만 추방시키는 eviction policy를 가지고 있습니다. 따라서 모든 프레임들의 reference count가 2 이상인 경우의 케이스를 다룰 수가 없습니다. 하나의 프레임을 공유하는 프로세스 관계가 일대다인 경우를 위해서 구조를 변경할 필요가 있습니다.
	- 네 사용중인 eviction policy는 evict가 불가능해지는 상황이 가능해지므로 고치는게 나아 보입니다.
