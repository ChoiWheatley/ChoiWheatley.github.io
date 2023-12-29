---
aliases: 
tags:
  - scrap
description: 
title: The C10K problem
created: 2023-09-22T10:28:03
updated: 2023-09-22T10:31:27
---
<http://www.kegel.com/c10k.html>

## README

socket 통신이 나오게 된 배경에 대해서 다룬다.

## GPT abstraction

ChatGPT (Web):

이 웹 페이지의 핵심 내용을 분석하고 요약하면 다음과 같습니다:

이 페이지는 FreeBSD 및 이후에는 NetBSD를 위한 권장되는 엣지 트리거형 폴 대체인 kqueue()에 대한 정보를 제공합니다. FreeBSD 4.3 이후 버전 및 2002년 10월 현재의 NetBSD는 poll()의 일반화된 대안인 kqueue()/kevent()를 지원하며 엣지 트리거링과 레벨 트리거링을 모두 지원합니다. kqueue()을 호출하여 수신 개체를 할당하고 /dev/poll을 열기 대신에 하나를 할당합니다. 수신 대기 중인 이벤트를 변경하거나 현재 이벤트 목록을 가져오려면 kqueue()에서 반환된 기술자에 대해 kevent()를 호출합니다. 이는 소켓 대기 상태뿐만 아니라 일반 파일 대기 상태, 시그널 및 I/O 완료까지 모두 듣을 수 있습니다.

또한, FreeBSD의 스레딩 라이브러리와 kqueue() 간의 상호 작용에 문제가 있을 수 있음을 언급합니다.

또한 Linux 2.6 커널에 대한 권장 엣지 트리거형 폴 대체로서 Davide Libenzi가 제안한 /dev/epoll에 대한 정보를 제공합니다. Epoll은 리얼타임 시그널 대기 상태 알림과 유사하지만 중복 이벤트를 통합하고 대규모 이벤트 검색을 위한 더 효율적인 방법을 제공합니다. Epoll은 2.5 커널 트리에 2.5.46에서 인터페이스가 특별한 파일에서 시스템 호출인 sys_epoll로 변경된 후 병합되었습니다.

또한 다른 이벤트 소스를 통합하는 논의와 Polyakov의 kevent(Linux 2.6+)에 대한 정보도 제공합니다.

이 페이지에는 다양한 예제 및 라이브러리가 포함되어 있으며, 이러한 이벤트 알림 체계를 상호 교환 가능하게 사용하는 방법을 설명하는 링크도 제공합니다.
