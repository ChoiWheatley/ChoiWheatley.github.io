---
aliases: 
tags: 
description:
title: fdisk 사용 방법
created: 2024-09-17T18:12:32
updated: 2024-09-17T18:17:48
---
`fdisk`는 리눅스에서 주로 사용하는 디스크 파티션 관리 도구입니다. `fdisk`는 특히 MBR(Master Boot Record) 파티션 테이블을 관리하는 데 자주 사용되지만 GPT(GUID Partition Table)도 관리할 수 있습니다. 여기서는 파티션을 나누고 디스크를 포맷하는 데 필요한 기본적인 `fdisk` 사용법을 설명하겠습니다.

### 1. **`fdisk` 실행하기**

먼저, 디스크 장치 목록을 확인한 후 `fdisk`를 실행해야 합니다.

```bash
lsblk
```

또는

```bash
fdisk -l
```

위 명령어를 사용하여 시스템에 연결된 디스크와 파티션을 확인할 수 있습니다. 디스크가 `/dev/sda`, `/dev/sdb` 등으로 나타납니다.

```bash
sudo fdisk /dev/sdX  # 여기서 'X'는 관리하고자 하는 디스크의 장치 이름입니다 (예: /dev/sda).
```

### 2. **현재 파티션 테이블 확인**

`fdisk`가 실행되면 명령어 모드로 진입합니다. 먼저 현재 파티션 상태를 확인하려면 `p` 명령어를 입력합니다.

```bash
Command (m for help): p
```

이 명령어는 디스크의 파티션 정보를 출력합니다.

### 3. **새 파티션 생성하기**

새로운 파티션을 생성하려면 `n` 명령어를 사용합니다.

```bash
Command (m for help): n
```

`n`을 입력하면 새 파티션을 생성하는 대화형 옵션이 나타납니다:

- **Primary or Extended Partition**: 기본(primary) 파티션 또는 확장(extended) 파티션을 선택합니다. 보통은 기본 파티션(`p`)을 선택합니다.
- **파티션 번호**: 파티션 번호를 지정합니다 (보통 1에서 4 사이).
- **시작 섹터**: 시작 섹터를 지정합니다. 기본값을 사용하려면 그냥 Enter를 누르면 됩니다.
- **끝 섹터 (또는 파티션 크기)**: 끝 섹터를 지정하거나 `+100G`와 같이 크기를 지정합니다. 예를 들어, `+20G`는 20GB의 파티션을 생성합니다.

### 4. **파티션 삭제하기**

기존 파티션을 삭제하려면 `d` 명령어를 사용합니다.

```bash
Command (m for help): d
```

파티션 번호를 선택하여 삭제할 수 있습니다.

### 5. **변경 사항 저장하기**

파티션 테이블을 수정한 후, 변경 사항을 디스크에 저장하려면 `w` 명령어를 사용합니다. 이 명령어는 파티션 테이블을 쓰고 `fdisk`를 종료합니다.

```bash
Command (m for help): w
```

만약 변경 사항을 저장하지 않고 종료하려면 `q` 명령어를 사용하여 종료합니다.

```bash
Command (m for help): q
```

### 6. **파일 시스템 포맷하기**

새로 만든 파티션을 사용하려면 해당 파티션을 포맷해야 합니다. 일반적으로 `mkfs` 명령어를 사용하여 파일 시스템을 생성합니다.

- **ext4 파일 시스템으로 포맷하기**:

```bash
sudo mkfs.ext4 /dev/sdX1  # X는 디스크 장치, 1은 파티션 번호
```

- **xfs 파일 시스템으로 포맷하기**:

```bash
sudo mkfs.xfs /dev/sdX1
```

이 명령어를 실행하면 해당 파티션이 지정된 파일 시스템으로 포맷됩니다.

### 7. **파티션 마운트**

포맷이 완료되면 해당 파티션을 마운트하여 사용할 수 있습니다. 마운트할 디렉터리를 만들고 `mount` 명령어를 사용합니다.

```bash
sudo mkdir /mnt/my_partition
sudo mount /dev/sdX1 /mnt/my_partition
```

### 8. **부팅 시 자동 마운트 설정**

파티션을 매번 수동으로 마운트하지 않으려면 `/etc/fstab` 파일에 자동 마운트 설정을 추가할 수 있습니다. `blkid` 명령어로 파티션의 UUID를 확인한 후 `/etc/fstab`에 다음과 같이 추가합니다:

```bash
sudo blkid /dev/sdX1

sudo nano /etc/fstab

# 아래와 같은 형식으로 라인 추가
UUID=your-uuid-from-blkid  /mnt/my_partition  ext4  defaults  0  0
```

---

이 과정을 통해 `fdisk`를 사용하여 파티션을 나누고, 파일 시스템을 포맷하며, 디스크를 사용할 준비를 마칠 수 있습니다.
