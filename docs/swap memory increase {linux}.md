---
aliases: 
tags: 
description:
title: swap memory increase {linux}
created: 2023-09-28T17:38:01
updated: 2024-09-19T00:51:52
---

## README

본 방식은 스왑파일을 만들어 스왑 공간을 할당하는 방식입니다. 이 방식 말고도 [[LVM을 사용한 swap 공간 할당 방법]] 도 있고, [[swap partition]] 도 있으니 참고 바랍니다.

---
- [[0110 Utility 🔧#linux utils]]
- <https://stackoverflow.com/questions/17173972/how-do-you-add-swap-to-an-ec2-instance>
- <https://velog.io/>@shawnhansh/AWS-EC2-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%8A%A4%EC%99%91>

```shell
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=2048
sudo /sbin/mkswap /var/swap.1
sudo chmod 600 /var/swap.1
sudo /sbin/swapon /var/swap.1
```

`/etc/fstab/`을 열고 마지막 줄에 아래의 내용을 추가하여 부팅시에 스왑파일을 정의한다.

```
/var/swap.1 swap swap defaults 0 0
```

스왑 적용이 잘 됐는지 확인하는 명령어 : `free` 

```
               total        used        free      shared  buff/cache   available
Mem:        24616228     2226216    21492316       16560      897696    22018784
Swap:       10485760           0    10485760
```
