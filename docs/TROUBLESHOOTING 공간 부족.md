---
aliases: 
tags: 
description:
title: TROUBLESHOOTING 공간 부족
created: 2024-09-17T22:20:37
updated: 2024-09-17T22:21:06
---

`lv_root`의 공간을 늘리고 `lv_home`의 공간을 줄이려면 LVM에서 논리 볼륨 크기를 조정하는 작업을 수행해야 합니다. 이 과정에서는 주의가 필요하며, 파일 시스템의 크기를 먼저 조정한 다음 논리 볼륨 크기를 변경해야 합니다. 아래 단계는 `lv_home`의 크기를 줄이고, `lv_root`의 크기를 늘리는 방법을 설명합니다.

### 1. **`lv_home`의 크기를 줄이기**

#### 1.1. **`lv_home`의 파일 시스템 크기 줄이기**

논리 볼륨 크기를 줄이기 전에 먼저 그 위에 설치된 파일 시스템의 크기를 줄여야 합니다. 파일 시스템을 줄이지 않고 논리 볼륨을 먼저 줄이면 데이터 손실이 발생할 수 있습니다. 

`lv_home`에 일반적으로 `ext4` 파일 시스템이 있을 가능성이 크므로, `resize2fs` 명령을 사용하여 파일 시스템 크기를 줄입니다.

```bash
sudo umount /dev/volgroup0/lv_home
sudo e2fsck -f /dev/volgroup0/lv_home  # 파일 시스템을 검사
sudo resize2fs /dev/volgroup0/lv_home 4G  # 파일 시스템을 4GiB로 줄이기
```

#### 1.2. **`lv_home` 논리 볼륨 크기 줄이기**

파일 시스템의 크기를 줄인 후, 이제 논리 볼륨 자체의 크기를 줄일 수 있습니다. 논리 볼륨을 4GiB로 줄이기 위해 다음 명령을 사용합니다.

```bash
sudo lvreduce -L 4G /dev/volgroup0/lv_home
```

#### 1.3. **`lv_home` 다시 마운트**

논리 볼륨 크기를 줄인 후 `lv_home`을 다시 마운트합니다.

```bash
sudo mount /dev/volgroup0/lv_home /mnt/home
```

### 2. **`lv_root`의 크기를 늘리기**

#### 2.1. **`lv_root`의 크기 늘리기**

`lv_home`의 크기를 줄여 남은 공간을 `lv_root`에 할당할 수 있습니다. `lv_root` 논리 볼륨의 크기를 4GiB로 늘리려면 다음 명령을 사용합니다.

```bash
sudo lvextend -L +2G /dev/volgroup0/lv_root  # 현재 크기에서 2GiB 증가
```

#### 2.2. **`lv_root` 파일 시스템 확장**

논리 볼륨의 크기를 확장한 후, 그 위에 설치된 파일 시스템도 크기를 확장해야 합니다. `resize2fs` 명령을 사용하여 파일 시스템을 논리 볼륨의 크기만큼 확장합니다.

```bash
sudo resize2fs /dev/volgroup0/lv_root
```

### 요약

1. `lv_home`의 파일 시스템 크기를 먼저 줄인 후, 논리 볼륨의 크기를 4GiB로 줄입니다.
2. 줄인 공간을 활용해 `lv_root` 논리 볼륨의 크기를 2GiB에서 4GiB로 확장한 후, 파일 시스템도 확장합니다.

이 과정을 통해 `lv_root`의 공간을 성공적으로 늘리고, `lv_home`의 크기를 줄일 수 있습니다.
