---
aliases: 
tags: 
description:
title: fdisk 읽는 방법
created: 2024-09-16T16:47:50
updated: 2024-09-16T16:48:53
---

> [!question] 내 컴퓨터에 설치되어있는 SSD를 현재 파티션을 두개로 나누어 하나는 윈도우를, 하나는 리눅스 운영체제를 설치했어. 지금 리눅스 운영체제가 설치되어있는 파티션을 초기화하려고 하는데, 어떤 볼륨이 어떤 운영체제에 설치되어있는지 알 수 있는 방법 알려줘

리눅스와 윈도우가 각각 어떤 파티션에 설치되어 있는지 확인하려면 몇 가지 방법을 사용할 수 있습니다. 각 운영체제에서 파티션을 확인하는 방법을 알려드릴게요.

### 1. **Windows에서 파티션 확인**

Windows에서 디스크 관리 도구를 사용하여 확인할 수 있습니다.

1. **디스크 관리 실행**:
   - `Win + X`를 누른 후 "디스크 관리"를 선택합니다.
   
2. **파티션 확인**:
   - 디스크 관리 창에서 각 디스크와 파티션이 어떻게 구성되어 있는지 볼 수 있습니다.
   - Windows가 설치된 파티션은 일반적으로 "C:" 드라이브로 표시됩니다.
   - 리눅스 파티션은 Windows에서 파일 시스템을 인식하지 못하기 때문에 "알 수 없음" 또는 "RAW"로 표시될 수 있습니다.

### 2. **Linux에서 파티션 확인**

리눅스에서 터미널을 사용하여 어떤 파티션에 운영체제가 설치되어 있는지 확인할 수 있습니다.

1. **터미널 열기**:  
   리눅스에서 터미널을 엽니다.

2. **`lsblk` 명령 사용**:

   ```
   sudo lsblk
   ```

   이 명령을 입력하면 현재 연결된 디스크와 파티션, 마운트된 경로가 표시됩니다. 리눅스 파티션은 "/"나 "/home" 등으로 마운트되어 있을 것입니다.

3. **`fdisk` 명령 사용**:

   ```
   sudo fdisk -l
   ```

   이 명령은 디스크와 파티션 정보를 자세하게 보여줍니다. 파티션의 파일 시스템 형식도 확인할 수 있습니다. Windows 파티션은 `ntfs`나 `fat32`로 표시되고, 리눅스 파티션은 `ext4`, `swap` 등으로 표시됩니다.

4. **`df` 명령 사용**:

   ```
   df -h
   ```

   이 명령은 각 파일 시스템의 마운트 지점과 용량을 보여줍니다. 리눅스 루트 파티션은 "/"로 마운트되어 있을 가능성이 높습니다.

이 방법들 중 하나를 사용하여 각 파티션에 어떤 운영체제가 설치되어 있는지 확인한 후 리눅스 파티션을 초기화할 수 있습니다.

> [!question] 아래는 내가 `fdisk -l` 명령어를 통해 출력된 결과야. 이 결과물을 해석해줘:

```
Disk /dev/loop0: 4 KiB, 4096 bytes, 8 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop1: 55.66 MiB, 58363904 bytes, 113992 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop2: 55.66 MiB, 58363904 bytes, 113992 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop3: 63.95 MiB, 67051520 bytes, 130960 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop4: 63.95 MiB, 67051520 bytes, 130960 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop5: 74.24 MiB, 77844480 bytes, 152040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop6: 74.27 MiB, 77881344 bytes, 152112 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop7: 262.5 MiB, 275251200 bytes, 537600 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/nvme0n1: 465.76 GiB, 500107862016 bytes, 976773168 sectors
Disk model: SHGP31-500GM                            
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 772125A0-E9C2-4E60-B934-B523D25C4FD5

Device             Start       End   Sectors   Size Type
/dev/nvme0n1p1      2048    923647    921600   450M Windows recovery environment
/dev/nvme0n1p2    923648   1128447    204800   100M EFI System
/dev/nvme0n1p3   1128448   1161215     32768    16M Microsoft reserved
/dev/nvme0n1p4   1161216 975060991 973899776 464.4G Microsoft basic data
/dev/nvme0n1p5 975060992 976773119   1712128   836M Windows recovery environment


Disk /dev/nvme1n1: 465.76 GiB, 500107862016 bytes, 976773168 sectors
Disk model: SHGP31-500GM                            
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 7675CDF3-192D-4797-B724-43CC11883BC6

Device             Start       End   Sectors   Size Type
/dev/nvme1n1p1        34     32767     32734    16M Microsoft reserved
/dev/nvme1n1p2     32768 714627071 714594304 340.7G Microsoft basic data
/dev/nvme1n1p3 714627072 976771071 262144000   125G Linux filesystem


Disk /dev/sda: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
Disk model: HGST HTS721010A9
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: DF39EF89-332B-421B-8037-6E352CA17857

Device        Start        End    Sectors   Size Type
/dev/sda1      2048    1050623    1048576   512M EFI System
/dev/sda2  29296640   33202175    3905536   1.9G Linux swap
/dev/sda3  33202176   95703039   62500864  29.8G Linux filesystem
/dev/sda4  95703040 1953521663 1857818624 885.9G Microsoft basic data
/dev/sda5   1050624   29296639   28246016  13.5G Linux filesystem

Partition table entries are not in disk order.


Disk /dev/loop8: 266.63 MiB, 279584768 bytes, 546064 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop9: 10.69 MiB, 11206656 bytes, 21888 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop10: 10.72 MiB, 11239424 bytes, 21952 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop11: 164.82 MiB, 172830720 bytes, 337560 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop12: 496.98 MiB, 521121792 bytes, 1017816 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop13: 504.15 MiB, 528642048 bytes, 1032504 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop14: 91.69 MiB, 96141312 bytes, 187776 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop15: 84.33 MiB, 88424448 bytes, 172704 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop16: 84.32 MiB, 88416256 bytes, 172688 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop17: 25.35 MiB, 26583040 bytes, 51920 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop18: 27.38 MiB, 28712960 bytes, 56080 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop19: 10.23 MiB, 10731520 bytes, 20960 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop20: 10.25 MiB, 10743808 bytes, 20984 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop21: 38.73 MiB, 40615936 bytes, 79328 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop22: 38.83 MiB, 40714240 bytes, 79520 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop23: 452 KiB, 462848 bytes, 904 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop24: 63.99 MiB, 67100672 bytes, 131056 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop25: 63.9 MiB, 67006464 bytes, 130872 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

출력된 `fdisk -l` 결과를 해석해보면, 여러 디스크와 파티션의 정보가 나열되어 있습니다. 중요한 부분을 해석해드리겠습니다.

### 주요 디스크들:

1. **/dev/nvme0n1 (500GB SSD)**
   - 이 디스크는 **Windows** 운영체제가 설치된 것으로 보입니다.
   - 파티션 정보:
     - **/dev/nvme0n1p1**: 450MB, Windows 복구 환경 (Recovery)
     - **/dev/nvme0n1p2**: 100MB, EFI 시스템 파티션 (부팅 관련)
     - **/dev/nvme0n1p3**: 16MB, Microsoft 예약 파티션 (Windows 설치 시 자동 생성)
     - **/dev/nvme0n1p4**: 464.4GB, Microsoft 기본 데이터 파티션 (Windows가 설치된 메인 파티션)
     - **/dev/nvme0n1p5**: 836MB, Windows 복구 환경

2. **/dev/nvme1n1 (500GB SSD)**
   - 이 디스크는 **리눅스** 운영체제가 설치된 것으로 보입니다.
   - 파티션 정보:
     - **/dev/nvme1n1p1**: 16MB, Microsoft 예약 파티션
     - **/dev/nvme1n1p2**: 340.7GB, Microsoft 기본 데이터 파티션 (이 파티션은 Windows 데이터가 포함될 수 있음)
     - **/dev/nvme1n1p3**: 125GB, 리눅스 파일 시스템 (이 파티션에 리눅스가 설치된 것으로 보입니다)

3. **/dev/sda (1TB HDD)**
   - 이 디스크는 여러 파티션으로 나뉘어 있으며, 일부는 리눅스 관련 파티션으로 보입니다.
   - 파티션 정보:
     - **/dev/sda1**: 512MB, EFI 시스템 파티션
     - **/dev/sda2**: 1.9GB, Linux 스왑 (리눅스 시스템에서 사용하는 스왑 파티션)
     - **/dev/sda3**: 29.8GB, 리눅스 파일 시스템 (리눅스 운영체제가 설치되었을 수 있음)
     - **/dev/sda4**: 885.9GB, Microsoft 기본 데이터 파티션 (Windows 데이터 저장 용도로 보임)
     - **/dev/sda5**: 13.5GB, 리눅스 파일 시스템 (추가적인 리눅스 파일 저장소일 가능성 있음)

### `loop` 장치들:

- **/dev/loopX** 장치들은 일반적으로 시스템의 스냅 패키지 또는 임시 가상 파일 시스템과 관련된 것입니다. 실질적인 물리적 디스크와는 관련이 없으며 무시해도 됩니다.

### 결론:

- **/dev/nvme0n1**: Windows 운영체제가 설치된 디스크입니다.
- **/dev/nvme1n1**: 리눅스 운영체제가 설치된 디스크입니다. 특히 **/dev/nvme1n1p3** 파티션이 리눅스 파일 시스템으로 보입니다.
- **/dev/sda**: 혼합된 파티션이 있는 디스크로, 일부는 리눅스 스왑 및 파일 시스템, 일부는 Windows 데이터가 저장된 파티션입니다.

리눅스 운영체제를 삭제하거나 초기화하려면 **/dev/nvme1n1p3** 파티션과 **/dev/sda**의 리눅스 관련 파티션들(**/dev/sda2, /dev/sda3, /dev/sda5**)을 포맷하거나 삭제하시면 됩니다.
