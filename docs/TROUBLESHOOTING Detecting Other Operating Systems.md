---
aliases: 
tags: 
description:
title: TROUBLESHOOTING Detecting Other Operating Systems
created: 2024-09-19T02:14:18
updated: 2024-09-19T02:14:59
---
[wiki.archlinux / Detecting other operating systems](https://wiki.archlinux.org/title/GRUB#Detecting_other_operating_systems)

---

> [!question] grub-mkconfig 명령을 사용하여 grub 부트로더 설정파일을 생성하는데 아래와 같은 경고 메시지가 떴어. os-prober 패키지를 설치는 했지만 grub에는 적용이 안된 것 같은데 해결 방법 좀 알려줘:

```
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
```

---

이 경고 메시지는 `os-prober` 패키지가 설치되었지만, **GRUB**이 다른 부팅 가능한 파티션을 자동으로 탐지하지 않도록 설정되어 있다는 의미입니다. 즉, 다른 운영체제(예: Windows, 다른 리눅스 배포판 등)를 GRUB 메뉴에 자동으로 추가하지 않도록 되어 있다는 경고입니다.

### 문제의 원인:

GRUB 2.06 버전부터는 보안 문제를 방지하기 위해 **`os-prober`**의 자동 탐지가 기본적으로 비활성화되었습니다. 따라서 `os-prober` 패키지를 설치해도 GRUB 설정에서 이를 명시적으로 활성화하지 않으면, 다른 부팅 가능한 파티션을 탐지하지 않습니다.

### 해결 방법:

1. **GRUB 설정 파일 수정:**  
   먼저, `os-prober` 기능을 다시 활성화하기 위해 GRUB 설정 파일(`/etc/default/grub`)을 수정해야 합니다.

   다음 명령어로 설정 파일을 엽니다:

   ```bash
   sudo nano /etc/default/grub
   ```

2. **`GRUB_DISABLE_OS_PROBER=false` 설정 추가:**  
   파일의 적절한 위치에 다음 줄을 추가하세요:

   ```bash
   GRUB_DISABLE_OS_PROBER=false
   ```

   이 설정은 GRUB이 다른 운영체제를 탐지하고, 부트 메뉴에 추가하도록 만듭니다.

3. **GRUB 설정 파일 재생성:**  
   파일을 저장하고 나서, GRUB 설정 파일을 다시 생성합니다. 다음 명령어를 실행하세요:

   ```bash
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

   이 명령어가 실행되면 `os-prober`가 실행되어 다른 부팅 가능한 파티션들을 탐지하고, GRUB 부트 메뉴에 추가됩니다.

4. **시스템 재부팅:**  
   설정이 완료되면 시스템을 재부팅하여 GRUB 메뉴에 다른 운영체제가 표시되는지 확인합니다.

### 요약:

이 문제는 GRUB 2.06에서 보안 강화를 위해 `os-prober`가 기본적으로 비활성화되었기 때문입니다. 이를 해결하려면 `/etc/default/grub`에 `GRUB_DISABLE_OS_PROBER=false`를 추가하고, GRUB 설정 파일을 다시 생성하면 됩니다.

이 과정 후에도 문제가 있거나 더 궁금한 사항이 있으면 알려주세요!
