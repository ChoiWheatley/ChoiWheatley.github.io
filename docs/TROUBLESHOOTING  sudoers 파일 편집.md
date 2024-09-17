---
aliases: 
tags: 
description:
title: TROUBLESHOOTING  sudoers 파일 편집
created: 2024-09-17T23:53:32
updated: 2024-09-17T23:53:50
---

> [!question] 아치리눅스에서 useradd로 생성한 유저가 sudoer가 아니라고 하는데, 아래처럼 유저에게 wheel 그룹을 부여했거든? 어떻게 된 일일까?

```
useradd -m -g users -G wheel choiwheatley
```

---

유저에게 `wheel` 그룹을 부여한 것은 올바른 과정이지만, 아치 리눅스에서 `wheel` 그룹의 사용자가 `sudo` 명령을 사용할 수 있도록 별도의 설정이 필요합니다. 이를 위해서는 `/etc/sudoers` 파일에서 `wheel` 그룹에 대한 권한을 명시적으로 활성화해야 합니다.

다음 절차를 따라 해결할 수 있습니다:

1. **`visudo` 명령어로 sudoers 파일을 편집**:  
   먼저 `visudo` 명령어로 `/etc/sudoers` 파일을 엽니다. `visudo`를 사용하는 이유는 이 명령이 파일을 편집하는 도중에 발생할 수 있는 오류를 방지하기 위해서입니다.

   ```bash
   visudo
   ```

2. **`wheel` 그룹에 대한 권한 활성화**:  
   열려 있는 `/etc/sudoers` 파일에서 아래와 같은 줄을 찾아 주석을 제거합니다.

   ```bash
   ## Uncomment to allow members of group wheel to execute any command
   # %wheel ALL=(ALL:ALL) ALL
   ```

   위의 줄에서 주석(`#`)을 제거하여 다음과 같이 만듭니다.

   ```bash
   %wheel ALL=(ALL:ALL) ALL
   ```

3. **파일 저장 후 종료**:  
   파일을 수정한 후 저장하고 편집기를 종료합니다. 이로써 `wheel` 그룹에 속한 사용자들이 `sudo` 권한을 사용할 수 있게 됩니다.

4. **유저 권한 확인**:  
   설정 후에 `choiwheatley` 유저로 로그인하고 `sudo` 명령어가 정상적으로 동작하는지 확인합니다.

이 과정을 거치면 `wheel` 그룹에 속한 사용자가 `sudo` 명령을 사용할 수 있게 될 것입니다.
