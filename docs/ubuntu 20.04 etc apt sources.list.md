---
description:
aliases: 
tags: 
created: 2023-04-08T08:54:04
updated: 2023-07-15T21:33:03
title: ubuntu 20.04 etc apt sources.list
---
- https://gist.github.com/ishad0w/788555191c7037e249a439542c53e170
- 실수로 apt 리포지토리 설정을 건들였다가 무언가 심각하게 잘못되었음을 알게 된 가엾은 나에게 주어진 단물같은 파일...
- `/etc/apt/sources.list` 파일에 다음을 추가하고

```
deb http://archive.ubuntu.com/ubuntu/ focal main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal-updates main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal-backports main restricted universe multiverse

deb http://archive.canonical.com/ubuntu focal partner
deb-src http://archive.canonical.com/ubuntu focal partner
```

- `sudo apt update && sudo apt upgrade` 를 실행하면 된다.
