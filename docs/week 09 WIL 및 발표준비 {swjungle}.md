---
aliases: 
tags: 
description:
title: week 09 WIL 및 발표준비 {swjungle}
created: 2023-10-23T21:55:23
updated: 2023-10-28T17:19:19
---
- [[week09 - Virtual Memory {pintos} {swjungle}]]
---
![[Programming Lesson Infographics by Slidesgo의 사본_page-0001.jpg]]  
include 순서를 바꾸다 마주한 컴파일 에러 메시지를 분석하고 문제의 원인을 파악한 과정을 발표.

![[Programming Lesson Infographics by Slidesgo의 사본_page-0002.jpg]]  
순환참조와 헤더가드로 타입 선언이 이루어지지 않은경우 + incomplete type에 대해서 발표

![[Programming Lesson Infographics by Slidesgo의 사본_page-0003.jpg]]  
포매터를 사용하면 다음과 같이 알파벳 순서로 include를 정렬하게 된다. 이때 다음과 같은 컴파일 에러메시지를 발견하게 됨.

![[Programming Lesson Infographics by Slidesgo의 사본_page-0004.jpg]]  
include는 단순히 해당 파일의 내용을 치환하는 것에 불과. 이 경우 동시에 여러 파일이 하나의 헤더를 include하게 되면 중복선언 문제가 발생할 수 있음. 따라서 Header Guard를 설정하여 단 한번만 선언되게 만들 수 있다.

![[Programming Lesson Infographics by Slidesgo의 사본_page-0005.jpg]]  
헤더가드 때문에 순환참조 발생시 심볼 참조가 원하는 대로 이루어지지 않을 수 있다.

  

1. uninit.c는 uninit.h를 include한다.

2. uninit.h를 읽을땐 VM_UNINT_H가 정의되어있지 않아 이를 정의하고 헤더가드를 통과함. 

3. 그리고 vm.h를 include하게 되는데…

4. vm.h는 다시 uninit.h를 include하여 순환참조가 발생한다. 이 시점 컴파일러는 VM_UNINIT_H가 정의되어있기 때문에 uninit.h의 내용을 건너뛴다. 

5,6. 아직 vm_initializer는 선언되지 않았다! 이 상태에서 vm_initializer를 인자로 갖는 함수 vm_alloc_with_initalizer를 선언한다.

> 이때 컴파일 에러가 발생함.

1. 컴파일 에러가 발생하지 않았다고 가정할때, vm.h의 내용을 uninit.h에 치환하고 난 뒤에 비로소 vm_initializer타입이 선언이 된다.

![[Programming Lesson Infographics by Slidesgo의 사본_page-0006.jpg]]  
따라서, vm.h를 먼저 include 하여야 vm_initializer를 먼저 선언할 수 있습니다.

![[Programming Lesson Infographics by Slidesgo의 사본_page-0007.jpg]]  
전방 선언을 통한 함수 활용을 예시로 들어보겠습니다.

함수(function)는 반환값, 인자의 개수 및 종류, 저장 식별자(extern, static, register) 같은 최소한의 정보만으로 사용 가능하다.

-> 이를 함수 시그니처 (function signature)라고 한다.

-> 함수는 주어진 인자를 받아 일련의 작업을 거치고 정해진 타입의 값을 반환한다. 이 작업에서는 이름은 필요없고 위에서 언급한 몇몇 정보만을 필요로 한다.

-> 호출에 필요한 정보는 구현에 필요한 정보보다 적으므로 (선언 정보는 필요조건, 구현 정보는 충분조건) 구현 (Implement)을 나중에 해도 문제가 없다.

  

헤더파일을 추가하는 이유이기도 하다.

헤더파일에 선언되어있는 함수, 변수들을 `#include`를 통해서 현재 .c 파일에 전방선언하고, 이를 활용해서 프로그램을 작성한다.

![[Programming Lesson Infographics by Slidesgo의 사본_page-0008.jpg]]  
frame을 관리하기 위한 frame table을 list로 선언해서 사용했습니다.

보통은 list.h 헤더파일을 추가해야 하지만 아무 문제가 없었습니다.

이유를 찾기위해 헤더파일을 추적해보니 다음과 같이 process.h -> thread.h -> list.h 순으로 include가 진행되면서 사용에 문제가 없음을 확인했습니다.

  

제가 list가 아닌 다른 자료구조를 사용하려고 했다면, 필요없는 list에 대한 선언들이 추가되었을 것이고 이는 컴파일 타임증가, 실행파일 크기증가로 이어질 수 있습니다.

(두번째 경우에는 DLL(동적 라이브러리) 적용 여부에 따라서 증가하지 않을 수 있습니다.)

  

이처럼 헤더파일을 잘 관리하지 않으면 자원낭비와 더불어 내가 사용하는 함수가 어디서 왔는지 추적하지도 못하는 상황이 발생할 수 있습니다.

![[Programming Lesson Infographics by Slidesgo의 사본_page-0009.jpg]]  
구조체를 전방선언해서 사용하기는 어렵다. 

main 함수에서 구조체 human을 초기화 하여 사용하려고 하지만 이 작업은 불가능하다.

구조체의 멤버 타입과 크기를 알 수 없기 때문이다.

(A structure type whose members you have not yet specified.), //  (information needed to determine the size of the identifier)

  

마찬가지로 유니온도 전방선언이 불가능하며

정확한 크기의 정의가 없는 배열 ex) `char arr[]` 도 사용이 불가능하다.
