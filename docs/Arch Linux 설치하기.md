---
aliases: 
tags: 
description:
title: Arch Linux 설치하기
created: 2024-09-16T15:05:05
updated: 2024-09-19T03:12:24
---

## README

[youtube / Arch Linux Installation Guide 2024: An Easy to Follow Tutorial](https://www.youtube.com/watch?v=FxeriGuJKTM) 에서 Manual Installation 파트를 따라해보면서 리눅스 설치 과정을 개념적으로 이해하기 위해 만든 문서입니다. 따라서 설치 순서가 맞지 않을 수도 있으며, 어떤 정보는 축약될 수도 있습니다.

## 인터넷 설정

[Network management](https://wiki.archlinux.org/title/Network_configuration#Network_management) 섹션을 확인하여 현재 시스템에 ip 주소가 할당되었는지 확인하세요.

## Partitioning Disks

`fdisk` 유틸리티 하에서 아치리눅스 디스크 파티션을 나눈다. 하나는 부팅을 위한 파티션 `/dev/vda1`으로, 하나는 EFI을 위한 파티션 `/dev/vda2`으로, 나머지는 일반 스토리지를 위해서 나눈다 `/dev/vda3`. 세번째 파티션의 타입을 44번, Linux LVM으로 설정해준다. 

세번째 파티션만 LVM으로 설정한 이유는, 이 파티션을 루트 볼륨과 홈 볼륨으로 논리적으로 나누어 사용할 예정이기 때문이다.

[[fdisk 사용 방법]]

[wiki / Logical Volume Manager](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux))

> [!question] [[lvm과 ext의 차이점이 뭐야?]]

> 결과물

![[Pasted image 20240916163134.png]]

### Format each partitions

각 파티션들의 포맷을 설정해줘야 한다. 

- `/dev/vda1`  ⇒ FAT32  포맷 ∵ EFI 시스템 파티션 (ESP) 에는 FAT32로 포맷하는 것이 적절하기 때문 [[UEFI 시스템과 BIOS 시스템]]

```
# mkfs.fat -F32 /dev/vda1
```

- `/dev/vda2` ⇒ EXT4 포맷 

```
# mkfs.ext4 /dev/vda2
```

> [!question] `/dev/vda3`는 왜 포맷 안해?

### Commit and format disks

`w` 명령을 사용하면 지금까지 적용한 세팅이 커밋되어 디스크 공간을 실제로 비우고 포맷팅하게 된다.

![[Pasted image 20240917194741.png]]

---

내 시스템에는 윈도우와 리눅스가 같이 설치되어있기 때문에 가상머신으로 했을때와는 다르게 볼륨리스트가 복잡하게 꼬여있을 것이다.

> [!question] 내 컴퓨터에 설치되어있는 SSD를 현재 파티션을 두개로 나누어 하나는 윈도우를, 하나는 리눅스 운영체제를 설치했어.  
> 지금 리눅스 운영체제가 설치되어있는 파티션을 초기화하려고 하는데, 어떤 볼륨이 어떤 운영체제에 설치되어있는지 알 수 있는 방법 알려줘

[[fdisk 읽는 방법]]

---

## Setting up an encrypted partition (Optional)

[wiki.archlinux / Cryptsetup usage](https://wiki.archlinux.org/title/Dm-crypt/Device_encryption#Cryptsetup_usage)

> [!caution] 주의! 이 작업을 수행할 시 매 부팅마다 비밀번호를 입력해야 합니다.

파티션을 암호화하기 위하여 cryptsetup luksFormat /dev/vda3을 입력한다. 비밀번호를 설정해야 한다. 그리고 암호화된 볼륨을 사용하기 위해선 cryptsetup open --type luks /dev/vda3 ${mapper-name}을 입력하여 복호화하면 된다.  mapper-name 을 추후에 사용하게 되니 기억하고 있자.  유튜버 따라 mapper-name은 lvm으로 넣어놓았다.

## Configuring LVM

[[LVM의 기본 구성 요소]]

### physical volume 연동하기

`pvcreate`는 physical volume create의 약어이다.  `pvcreate /dev/mapper/${mapper-name}

![[Pasted image 20240916170234.png]]

### volume group 등록하기

`vgcreate` 는 volume group create의 약어이다. `vgcreate volgroup0 /dev/mapper${mapper-name}`

### logical volume 등록하기

`lvcreate`는 logical volume create의 약어이다. 다음 두가지 명령이 필요하다:

- `lvcreate -L 20GB volgroup0 -n lv_root`: 루트디렉토리에 논리볼륨을 할당
- `lvcreate -L 128GB volgorup0 -n lv_home`: 홈 디렉토리에 논리볼륨을 할당
- (추가) `lvcreate -L 2GB volgroup0 -n lv_swap`: 스왑영역 할당 [[LVM을 사용한 swap 공간 할당 방법]] 참조

논리볼륨 확인하려면 `lvdisplay` 명령어를 치면 된다.

![[Pasted image 20240916210349.png]]

> 오? 그러면 볼륨그룹 보는 명령어는 `vgdisplay`이겠네?

![[Pasted image 20240916210505.png]]

### Load Device Mapper Module

[[dm_mod, Device Mapper Module]]

### Activate LVM Volume

일련의 두 명령어를 통하여 볼륨그룹을 식별하고 활성화까지 한다.

- `vgscan`
- `vgchange -ay`

[[lvm 설정하는데 vgscan과 vgchange가 필요한 이유]]

### 논리볼륨에 파일 시스템 연결하기

[[#Format each partitions]]에서 빼먹은 `/dev/vda3`에 대한 파티션 포맷을 각각의 LVM에 대해서 설정해줄 차례다.

[[lvm과 ext의 차이점이 뭐야?]]에서 알 수 있듯이, LVM은 하드웨어 스토리지를 추상화하고 EXT는 파일 구조를 추상화한 개념이라고 볼 수 있다. 따라서 LVM이 하드웨어에 더 가깝다고 볼 수 있음. 여튼 지금까지 공들여 만든 두 논리볼륨 `lv_root`, `lv_home`을 파일시스템 로직과 결합할 차례이다. 아래 두 명령어를 실행시키면 된다:

- `mkfs.ext4 /dev/volgroup0/lv_root`
- `mkfs.ext4 /dev/volgroup0/lv_home`


![[Pasted image 20240916213914.png]]

### 파일 시스템을 마운트하기

[[#Partitioning Disks]]에서 나눈 세개의 파티션을 기억하자. vda1은 EFI, vda2는 부팅, vda3는 홈 디렉터리를 위한 공간이다. 우리가 만든 논리볼륨과 파티션을 파일 디렉터리에 마운트를 하여 실제로 사용가능한 상태로 만들자. 참고로 없는 디렉토리는 그냥 `mkdir`로 생성하면 그만이다.

```
mount /dev/volgroup0/lv_root /mnt
mount /dev/vda2 /mnt/boot #mkdir 필요
mount /dev/volgroup0/lv_home /mnt/home #mkdir 필요
```

> [!question] [[논리볼륨과 파티션의 차이점]]

## Installing required packages

`pacstrap -i /mnt base` 명령어를 사용하여 아치리눅스가 필요로 하는 필수 패키지를 설치하자. 이때 `-i` 옵션은 interactive의 약어이다. 두 번 물어본다. 레포지토리 코어 제공자를 선택하는것과 설치할 패키지에 대해서 세심하게 물어봐주는 것이다.

![[Pasted image 20240916215850.png]]

## Generating the fstab file

파일 시스템 메타데이터를 fstab 파일에 저장하자. 아래 스크린샷은 생성된 `/mnt/etc/fstab` 파일을 볼 수 있다.

```
genfstab -U -p /mnt >> /mnt/etc/fstab
```

![[Pasted image 20240916220239.png]]

> [!question] [[fstab 파일이 필요한 이유]]

## Arch Install Shell에 진입하기

`arch-chroot`를 사용하여 아치리눅스 설치 셸에 진입, 패키지 설정 및 유저 설정을 진행할 수 있다. 스크린샷에서 볼 수 있듯이, 프롬프트가 바뀐 것을 확인할 수 있다.

![[Pasted image 20240916221724.png]]

## 루트 유저와 일반 유저 세팅하기

- `passwd` 아무 옵션 없으면 이 명령어는 루트 유저의 비밀번호를 세팅하게 된다.
- `useradd -m -g users -G wheel choiwheatley` choiwheatley라는 일반 유저를 생성하고 sudo 명령어가 먹는 wheel group에 초대한다.

[[useradd 사용방법]]

- `passwd choiwheatley`명령어로 choiwheatley 유저에 대한 비밀번호를 설정한다.

### [[TROUBLESHOOTING  sudoers 파일 편집]]

## Installing additional packages

아치리눅스는 `pacman` 명령어를 사용하여 패키지를 관리한다. 일단 저 유튜버가 추천하는 패키지를 먼저 설치해보자.

[youtube / Linux Crash Course - The Pacman Command](https://www.youtube.com/watch?v=HD7jJEh4ZaM)

```
pacman -S base-devel dosfstools grub efibootmgr gnome gnome-tweaks lvm2 mtools nano networkmanager openssh os-prober sudo
```

### [[TROUBLESHOOTING 공간 부족]]

이번에는 리눅스 커널을 설치한다. 리눅스 커널과 헤더, 그리고 혹여나 발생할 수 있는 리눅스 커널과 아치 사이에 충돌이 발생하여 부팅이 되지 않을 경우를 대비하여 리눅스 LTS도 함께 설치한다.

```
pacman -S linux linux-headers linux-lts linux-lts-headers
```

그리고 하드웨어 펌웨어도 같이 설치해준다

```
pacman -S linux-firmware
```

### [[GPU driver 설치]]

## 램디스크 생성 및 설정

`/etc/mkinitcpio.conf`파일을 열어 `HOOKS` 선언부의 `filesystems` 앞에 다음 두 패키지를 추가한다:

- encrypt (optional [[#Setting up an encrypted partition (Optional)]] 참고)
- lvm2

```
HOOKS=(base udev autodetect microcode modconf kms keyboard keymap consolefont block encrypt lvm2 filesystems fsck)
```

[[램 디스크가 필요한 이유와 mkinitcpio.conf 와 HOOKS의 역할]]

그리고 설치한 리눅스 커널과 램디스크를 맞추기 위해 램디스크를 재생성한다:

```
mkinitcpio -p linux
mkinitcpio -p linux-lts
```

## 로캐일 설정

`/etc/locale.gen` 파일을 열어 원하는 로캐일을 찾아 주석해제한다.

![[Pasted image 20240917225259.png]]

그리고 `locale-gen` 명령을 사용하여 로캐일을 적용한다.

## GRUB

`/etc/default/grub` 파일을 수정한다. 우리는 `/dev/vda3`을 암호화했기 때문에 grub에서 이를 처리할 `cryptdevice`를 정의해주어야 한다.  `GRUB_CMDLINE_LINUX_DEFAULT` 라인을 수정하자:

(optional)  

```
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 cryptdevice=/dev/vda3:volgroup0 quiet"
```

### EFI 파티션 세팅

`/dev/vda1`를 부팅용으로 남겨두었었다. 그래서 `/dev/vda1`에 `/boot/EFI` 디렉토리를 마운트시켜주어야 한다.

```
mkdir /boot/EFI
mount /dev/vda1 /boot/EFI
```

### grub-install

grub 부트로더를 본격적으로 설치하자.

```
grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck
```

grub 부트로더 메시지 파일을 복사하여 grub이 영어메시지를 사용할 수 있도록 하자.

```
cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo
```

다음으로 grub 부트로더 설정파일을 생성한다. 참고로 grub-mkconfig 명령어는 `/etc/default/grup` 과 `/etc/grub.d/` 디렉토리의 스크립트를 기반으로 새로운 GRUB 설정 파일을 만든다.

```
grub-mkconfig -o /boot/grub/grub.cfg
```

### [[TROUBLESHOOTING Detecting Other Operating Systems]]

## Display Manager 구축

[archlinux.org / Display manager](https://wiki.archlinux.org/title/Display_manager)

**Gnome 을 사용할 경우**

GDM, AKA Gnome Display Manager

```
systemctl enable gdm
```

**xfce를 사용할 경우**

x-window 시스템 중에서 하나 고르면 된다. 예시로 xorg-xdm을 선택해보자.

```
pacman -S xdm-archlinux

systemctl enable xdm-archlinux.service
```

## NetworkManager 서비스 등록

```
systemctl enable NetworkManager
```

## REBOOT

이제 arch install shell을 나가고 reboot하면 된다.

![[Pasted image 20240917232550.png]]

음... Display Manager 일 안하네?

![[Pasted image 20240917232814.png]]

[archlinux.org / Xorg](https://wiki.archlinux.org/title/Xorg) 에서 `xorg-server` 패키지를 설치했더니 문제 해결됨.

![[Pasted image 20240917234120.png]]

음... `choiwheatley`가 sudoer가 아니라네?
