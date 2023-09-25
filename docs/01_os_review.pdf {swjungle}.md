---
aliases: 
tags: 
description:
title: 01_os_review.pdf {swjungle}
created: 2023-09-25T23:23:13
updated: 2023-09-26T01:36:42
---
- [01_os_review.pdf](https://drive.google.com/file/d/1v7ZT0uCqnSFQQY3jQsnXnCh9WHPpgQxZ/view)
- [[2023-09-26 권영진 교수님의 OS 강의 (1차) {swjungle}]]
___

## Provide ABSTRACTION to use hardware

![[Pasted image 20230925204947.png]]

- [`gettimeofday`](https://www.man7.org/linux/man-pages/man2/gettimeofday.2.html) epoch부터 지금까지 소요된 시간을 리턴한다.  
mmap을 엄청나게 큰 공간을 할당하고 memset한 시간을 출력. 똑같은 일을 두 번 했는데 시간차이가 유의미 할 정도로 크게 났다. -> 공간적 지역성 (locality)의 역할

- key roles of OS?
- What is abstraction?
	- 그다지 중요해 보이지 않는 것들을 감추어 단순하게 만드는 것
	- cpu, memory, storage는 각각 추상화 되어 virtualized cpu, virtual memroy, file이 되었다. 
- cpu 추상화
- 메모리 추상화 [[9. Virtual Memory]]  
	- 페이지를 기준으로 가상 주소 공간과 물리 공간을 매핑  
	- MMU의 도움 덕분!  
	- Where is the page tables stored?  >> cpu 내의 캐시메모리  
	- What are roles of software in page? >> utilization 향상? 논리적 무결성?  
	- what are roles of hardware? >> 물리적으로 보장된 데이터 저장 및 불러오기?
- 저장장치 추상화
	-  Location of data is identified by **(file system)**
	- OS subsystem maps the file to physical storage. Let's call it **(file???)** >> file이란 단어 자체가 추상화된 개념인데..?
	- How to create the mappings from file to storage? >> 연속적으로, 연결리스트로, 물리 블록을 가리키는 포인터들을 저장한 블록으로, 계층노드로
	- Where are the internal nodes in the index? | 인덱스의 내부노드(리프가 아닌 노드)는 어디에 있는가? >> 똑같이 스토리지 안에 있는거 아닌가? 이걸 메인 메모리로 캐싱하는 건 read할 때에서야 일어나는거 아님?
	- Does hardware help for the indexing? If yes, what is roles of hardware? If not, why? >> 스토리지 자체 캐시 플래시 드라이브가 있다고 들었는데 [page cache {wiki}](https://en.wikipedia.org/wiki/Page_cache) 에 따르면 disk controller 또는 disk array controller 안에 자체 RAM 혹은 NVRAM이 캐싱을 담당한다고 한다.
		- 그래서 첫번째 질문에 답을 하자면, 하드웨어는 인덱싱을 도와준다고 말할 수 있다. 그 하드웨어는 파일과 실제 디스크 위치 (섹터, 페이지)를 매핑한다.
	- When to allocate physical block? >> read
	- Any performance optimization for slow storage device? >> multi-level cache 말고 더 있으려나?
- File System overview
	- ![[Pasted image 20230925224614.png]]
	- Why VFS is required? >> 세상은 넓고 스토리지 디바이스를 만드는 회사는 많고 호환되지 않는 소프트웨어들이 넘쳐 흐르기 때문. 그래서 일반적으로 사용될 프로토콜이 필요해졌다. 레이어를 수직적으로 배치한 덕분에 하드웨어 벤더 명세에 독립적으로 파일을 읽고 쓸 수 있게 되었다.
	- What is buffer cache? And why does it required? >> 오잉? [DMA](https://en.wikipedia.org/wiki/Direct_memory_access)와는 다른건가
	- Problems of using buffer cache
		- ![[Pasted image 20230925225850.png]]
		- Which data is up-to-date? >> 타이밍에 따라 다르지 않나? 버퍼 기준 읽고 쓸 때의 타이밍을 보면...
			- read before: storage device
			- read after: Buffer
			- write before: Buffer
			- write after: storage device
		- When is the data persisted? >> before read, after write
		- Do you see problems? >> 동시에 하나의 데이터를 접근할 때 문제가 생기겠죠 뭐.
		- Consistency: Atomicity and Durability
		- Crash consistency: ordering: [`fsync(2)`](https://linux.die.net/man/2/fsync)
			- 디스크에 데이터를 쓰는 작업은 순서 트리를 가진다. 각 노드가 의미하는 건 뭐냐?

## Protection & Isolation

- protection
	- privileged instruction... HOW?
	- memory protection
	- interrupts
- address translation concept by MMU
	- 두 가지 작업으로 분리가 될 줄은 몰랐는데... 각각 Segmentation 과정과 Paging 과정으로 나뉜다.
	- [?] CSAPP 책에서 다룬 가상메모리는 segmentation 과정이 없다. CPU가 처음부터 가상 메모리를 요청해 오는게 아니었다는 건가?
	- Segment Descriptor (x86)
		- ![[Pasted image 20230926010140.png]]
	- Page table entry
		- ![[Pasted image 20230926010437.png]]
- Dual mode OS
- Isolation by protection domain
	- memory isolation: virtual memory address protection, switch virtual address space between process
	- file isolation: file permission system
	- access control method?
		- [DAC](https://en.wikipedia.org/wiki/Discretionary_access_control), [MAC](https://en.wikipedia.org/wiki/Mandatory_access_control) 이게머고?
			- 컴퓨터 보안에서 임의 액세스 제어(DAC)는 신뢰할 수 있는 컴퓨터 시스템 평가 기준[1](TCSEC)에 정의된 액세스 제어의 한 유형으로, 대상 및/또는 대상에 속한 그룹의 신원을 기반으로 대상에 대한 액세스를 제한하는 수단입니다. 특정 액세스 권한을 가진 주체가 다른 주체에게 해당 권한을 (간접적으로) 전달할 수 있다는 점에서 이러한 제어는 재량에 따른 것입니다(필수 액세스 제어에 의해 제한되지 않는 한). 
		- authentication of user and system (mutually distrust)
		- protected IPC
			- shared memory
			- pipe

## sharing resources

> But my machine has only a single CPU and limited memory So, processes must share the resources

- two types of sharing
	- space sharing
		- memory by virtual memory + space reclamation(replacement)
	- time sharing
		- cpu by scheduling

- scheduler 종류와 그 특징
	- FIFO: 가장 먼저 들어온 프로세스 먼저 실행
		- Pro: 구현이 간단함
		- Con: [convoy effect](https://en.wikipedia.org/wiki/Lock_convoy) 잦은 문맥전환으로 인한 응답지연
	- SJF 또는 [SJN](https://en.wikipedia.org/wiki/Shortest_job_next) 
		- Pro: 응답시간에서 탁월함
		- Con: 기아상태
	- Round Robin 시간을 쪼개놓고 공정하게 분배
		- Pro: 공정한 실행으로 높은 응답시간
		- Con: 
