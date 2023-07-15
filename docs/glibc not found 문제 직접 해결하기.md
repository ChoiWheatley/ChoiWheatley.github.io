---
description:
aliases: 
tags: 
created: 2023-04-18T18:56:11
updated: 2023-07-15T21:33:04
title: glibc not found 문제 직접 해결하기
---
- https://sosal.kr/1065

```bash
mkdir ~/glibc_install; cd ~/glibc_install 

wget http://ftp.gnu.org/gnu/glibc/glibc-2.14.tar.gz

tar zxvf glibc-2.14.tar.gz

cd glibc-2.14

mkdir build

cd build

../configure --prefix=/opt/glibc-2.14

make -j4

sudo make install

export LD_LIBRARY_PATH=/opt/glibc-2.14/lib
```

위 설명은 glibc_2.14 버전인데

http://ftp.gnu.org/gnu/glibc/ 에서 원하는 버전에 맞춰서

위 커맨드를 입력하면 해결됨.
