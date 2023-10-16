---
aliases: 
tags: 
description:
title: swap memory increase {linux}
created: 2023-09-28T17:38:01
updated: 2023-10-15T20:59:48
---
- [[0110 Utility 🔧#linux utils]]
- <https://stackoverflow.com/questions/17173972/how-do-you-add-swap-to-an-ec2-instance>
- <https://velog.io/@shawnhansh/AWS-EC2-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%8A%A4%EC%99%91>

```shell
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
sudo /sbin/mkswap /var/swap.1
sudo chmod 600 /var/swap.1
sudo /sbin/swapon /var/swap.1
```

스왑 적용이 잘 됐는지 확인하는 명령어 : `free` 

```
               total        used        free      shared  buff/cache   available
Mem:        24616228     2226216    21492316       16560      897696    22018784
Swap:       10485760           0    10485760
```
