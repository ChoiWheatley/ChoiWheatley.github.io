---
aliases: 
tags: 
description:
title: memory representation {SP} {week03}
created: 2023-08-30T16:56:00
updated: 2023-08-30T17:13:26
---
- parent: [[0016 Systems Programming {ssu2021-1st} 🐼]]
___

## at a glance

- memory is just a bits
- 문맥이 있어야 비트를 이해할 수 있다. 어떻게 2진수가 정수형이 되고, 부동소수점이 되고, 문자가 되며, 그들의 복합체인 구조체가 되어 데이터를 표현할 수 있는가?
- Padding and Alignment: 모든 시스템은 데이터를 워드 단위로 정렬을 한다.  
	- ![[스크린샷 2023-08-30 오후 5.00.54.png]]
- 포인터 산수: stride는 가리켜지는 타입의 원소 사이의 간격을 의미한다. 포인터가 배열처럼 deref가 되는 이유가 바로 stride 덕분이다. double형 포인터의 stride는 8, int형 포인터의 stride는 4 이렇게 알아서 굴러 들어가는거다.
- 포인터 타입: 포인터 타입은 컴파일러에게 포인터 stride를 제공하기 위해 있는거지, 결국 이진코드에는 전부 오프셋으로 치환된다는거. 그러니까 우리가 원한다면 유효하지 않은 포인터를 만들어 버릴 수도 있음.
- pointer daring: 포인터 끼리는 임의 캐스팅이 가능함. 특히 `void *`의 경우, 어떤 타입의 포인터도 다 허용한다.
- 동적할당: `malloc()` and `free()`
- 바이트 정렬순서: endianness
- 1의 보수, 2의 보수를 통한 정수형 표현
- 실수형(부동소수점) 표현
