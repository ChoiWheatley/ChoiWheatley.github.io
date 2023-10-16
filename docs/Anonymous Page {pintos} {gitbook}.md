---
aliases: 
tags: 
description:
title: Anonymous Page {pintos} {gitbook}
created: 2023-10-15T20:13:51
updated: 2023-10-16T00:36:12
---
- [[week09 - Virtual Memory {pintos} {swjungle}]]
---

## Lazy Loading (Demand Paging) Flow

길지만 과제문서에 있는 내용이 답이었다. 이걸 먼저 읽고 코드분석을 했었더라면...

> Since we have three page types, the initialization routine is different for each page. Though will be explained again in the sections below, here we provide a high-level view of the page initialization flow. First, `vm_alloc_page_with_initializer` is invoked when the kernel receives a new page request. The initializer will initialize a new page by allocating a page structure and setting appropriate initializer depending on its page type, and return the control back to the user program. As the user program executes, at a point, a page fault occurs because the program is trying to access a page which it believes to possess but the page has no contents yet. During the fault handling procedure, `uninit_initialize` is invoked and calls the initializer you set earlier. The initializer will be `anon_initializer` for anonymous pages and `file_backed_initializer` for file-backed pages.

**bogus fault**

1. uninitialized page
2. swapped out page
3. write-protected page (COW)

## Lazy Loading Executable

실행파일을 읽어 프레임에 적재하는 과정이 뒤로 미뤄졌다. `load_segment`는 elf 파일의 헤더를 단순히 그 위치만을 `vm_alloc_page_with_initializer(type, va, writable, init, aux)`를 사용해 spt에 uninit page 구조체를 등록하고 실제 페이지를 올리지는 않는다. page fault 발생시 `try_handle_fault`에서 spt에 인자로 들어온 가상주소를 검색하게 되고, 존재할 경우 제일먼저 `uninit_initialize`가 실행돼 페이지의 타입을 등록당시 설정한 타입으로 바꿔준다. operations를 바꾸고 union 타입 속성을 변경한다. 마지막으로 실제 프레임을 할당하기 위해서 `swap_in`을 호출하게 되고, 이는 `anon_initializer`와 실행파일인 경우에 한해서 `lazy_load_segment`를 호출하면 된다.

**`setup_stack`**

> The first stack page need not be allocated lazily  
> 최초의 스택 페이지는 지연로딩할 필요가 없습니다. (띠용)

**`supplemental_page_table_copy`**

- [?] 왜 spt를 복제한 뒤에 uninit page를 그 즉시 claim해야하지? claim하면 프레임을 만들어 페이지와 연결시키겠다는 거잖아.

**`supplemental_page_table_kill`**

spt를 순회하며 각각의 페이지에 대해서 `destroy(page)`를 호출하라. 이 매크로는 내부적으로 `page.operations.destroy(page)`를 호출한다.

## Page Cleanup

**`uninit_destroy`**

아직 한 번도 로딩이 안 된 페이지는 uninit 타입이라고 했다. 이 경우 vm type에 따라서 해제할 리소스가 달라진다는 점 인지하고 있자.

**`anon_destroy`**

페이지와 연결된 프레임을 해제하면 될 뿐이고 페이지는 다른놈이 해제해줄거임.

## After Anonymous Page implementation...

> Now all of the tests in project2 should be passed.

뭣이?
