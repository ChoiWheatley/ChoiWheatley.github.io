---
aliases: 
tags: 
description:
title: swap memory increase {linux}
created: 2023-09-28T17:38:01
updated: 2023-09-28T17:38:26
---
- [[0110 Utility 🔧#linux utils]]
- <https://stackoverflow.com/questions/17173972/how-do-you-add-swap-to-an-ec2-instance>

```shell
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
sudo /sbin/mkswap /var/swap.1
sudo chmod 600 /var/swap.1
sudo /sbin/swapon /var/swap.1
```
