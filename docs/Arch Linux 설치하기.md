---
aliases: 
tags: 
description:
title: Arch Linux 설치하기
created: 2024-09-16T15:05:05
updated: 2024-09-16T21:05:06
---

## README

[youtube / Arch Linux Installation Guide 2024: An Easy to Follow Tutorial](https://www.youtube.com/watch?v=FxeriGuJKTM) 에서 Manual Installation 파트를 따라해보면서 리눅스 설치 과정을 개념적으로 이해하기 위해 만든 문서입니다. 따라서 설치 순서가 맞지 않을 수도 있으며, 어떤 정보는 축약될 수도 있습니다.

## 인터넷 설정

[Network management](https://wiki.archlinux.org/title/Network_configuration#Network_management) 섹션을 확인하여 현재 시스템에 ip 주소가 할당되었는지 확인하세요.

## Partitioning Disks

`fdisk` 프롬프트 하에서 아치리눅스 디스크 파티션을 나눈다. 하나는 부팅을 위한 파티션으로, 하나는 ????를 위한 파티션으로, 나머지는 일반 스토리지를 위해서 나눈다. 세번째 파티션의 타입을 44번, Linux LVM으로 설정해준다. 

[wiki / Logical Volume Manager](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux))

> [!question] [[lvm과 ext의 차이점이 뭐야?]]

> 결과물

![[Pasted image 20240916163134.png]]

내 시스템에는 윈도우와 리눅스가 같이 설치되어있기 때문에 가상머신으로 했을때와는 다르게 볼륨리스트가 복잡하게 꼬여있을 것이다.

> [!question] 내 컴퓨터에 설치되어있는 SSD를 현재 파티션을 두개로 나누어 하나는 윈도우를, 하나는 리눅스 운영체제를 설치했어. 지금 리눅스 운영체제가 설치되어있는 파티션을 초기화하려고 하는데, 어떤 볼륨이 어떤 운영체제에 설치되어있는지 알 수 있는 방법 알려줘

[[fdisk 읽는 방법]]

## Setting up an encrypted partition

파티션을 암호화하기 위하여 `cryptsetup luksFormat /dev/${volume-name}`을 입력한다. 비밀번호를 설정해야 한다. 그리고 암호화된 볼륨을 사용하기 위해선 `cryptsetup open --type luks /dev${volume-name} ${mapper-name}`을 입력하여 복호화하면 된다.  mapper-name 을 추후에 사용하게 되니 기억하고 있자. 

## Configuring LVM

[[LVM의 기본 구성 요소]]

**physical volume 연동하기**

`pvcreate`는 physical volume create의 약어이다.  `pvcreate /dev/mapper/${mapper-name}

![[Pasted image 20240916170234.png]]

**volume group 등록하기**

`vgcreate` 는 volume group create의 약어이다. `vgcreate volgroup0 /dev/mapper${mapper-name}`

**logical volume 등록하기**

`lvcreate`는 logical volume create의 약어이다. 다음 두가지 명령이 필요하다:

- `lvcreate -L 4GB volgroup0 -n lv_root`: 루트디렉토리에 논리볼륨을 할당
- `lvcreate -L 128GB volgorup0 -n lv_home`: 홈 디렉토리에 논리볼륨을 할당

논리볼륨 확인하려면 `lvdisplay` 명령어를 치면 된다.

![[Pasted image 20240916210349.png]]

> 오? 그러면 볼륨그룹 보는 명령어는 `vgdisplay`이겠네?

![[Pasted image 20240916210505.png]]

**Load Device Mapper Module**

[[dm_mod, Device Mapper Module]]