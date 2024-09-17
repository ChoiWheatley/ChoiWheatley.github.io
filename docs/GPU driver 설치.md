---
aliases: 
tags: 
description:
title: GPU driver 설치
created: 2024-09-17T22:30:44
updated: 2024-09-17T22:30:54
---

`lspci` 명령을 통해 GPU를 확인한 후, 각 GPU에 맞는 패키지를 아치 리눅스(Arch Linux)에 설치하는 방법은 다음과 같습니다. GPU 제조사에 따라 적절한 드라이버 패키지를 선택해야 합니다.

### 1. **Intel GPU**

Intel GPU는 대체로 기본 커널에 드라이버가 포함되어 있으므로, 추가 드라이버 설치는 필요하지 않을 수 있습니다. 그러나 최신 기능을 사용하거나 성능을 높이기 위해 추가 패키지를 설치할 수 있습니다.

- **필수 패키지**:

  ```bash
  sudo pacman -S xf86-video-intel
  ```

- **Vulkan 패키지 (필요 시)**:

  ```bash
  sudo pacman -S vulkan-intel
  ```

### 2. **AMD GPU**

AMD GPU는 오픈 소스 드라이버인 `amdgpu` 드라이버를 기본적으로 사용하며, AMDGPU-PRO 드라이버는 주로 특별한 목적에 사용됩니다.

- **필수 패키지**:

  ```bash
  sudo pacman -S xf86-video-amdgpu
  ```

- **Vulkan 패키지**:

  ```bash
  sudo pacman -S vulkan-radeon
  ```

- **OpenCL 패키지 (필요 시)**:

  ```bash
  sudo pacman -S opencl-mesa
  ```

- AMDGPU-PRO 드라이버가 필요하다면, [AUR](https://aur.archlinux.org/)에서 설치해야 합니다:

  ```bash
  yay -S amdgpu-pro-installer
  ```

### 3. **NVIDIA GPU**

NVIDIA는 오픈 소스 드라이버와 폐쇄 소스 드라이버 두 가지 옵션이 있습니다. 대부분의 사용자에게는 폐쇄 소스 드라이버(`nvidia`)가 더 나은 성능을 제공합니다.

- **폐쇄 소스 드라이버 (최신 카드용)**:

  ```bash
  sudo pacman -S nvidia nvidia-utils
  ```

- **구형 GPU 용 드라이버**:  
  만약 구형 카드(NVIDIA 390.xx 시리즈 이하)를 사용하는 경우:

  ```bash
  sudo pacman -S nvidia-390xx nvidia-390xx-utils
  ```

- **Vulkan 패키지**:

  ```bash
  sudo pacman -S nvidia-utils
  ```

- **OpenCL 패키지 (필요 시)**:

  ```bash
  sudo pacman -S opencl-nvidia
  ```

### 4. **Virtual GPU (QEMU/KVM/VirtualBox 등)**

가상 머신에서 가상 GPU를 사용 중이라면, 다음과 같은 드라이버를 설치해야 할 수 있습니다.

- **QEMU/KVM 용 Virtio GPU**:

  ```bash
  sudo pacman -S xf86-video-qxl
  ```

- **VirtualBox 용**:

  ```bash
  sudo pacman -S virtualbox-guest-utils
  ```

- **VMware 용**:

  ```bash
  sudo pacman -S xf86-video-vmware
  ```

### 5. **추가 설정**

#### `mkinitcpio`를 통해 초기 램디스크 재생성

NVIDIA와 같은 일부 드라이버는 초기 램디스크 재생성이 필요할 수 있습니다. 설치 후 다음 명령을 사용하여 재생성합니다:

```bash
sudo mkinitcpio -P
```

#### Xorg 설정

대부분의 경우 Xorg는 자동으로 GPU를 올바르게 인식하지만, 수동 설정이 필요할 경우 `/etc/X11/xorg.conf.d/` 디렉토리에 GPU에 맞는 설정 파일을 작성할 수 있습니다.

이와 같이 GPU 종류에 따라 적합한 드라이버를 설치하고, 필요한 경우 추가적으로 Vulkan, OpenCL 등을 설정할 수 있습니다.
