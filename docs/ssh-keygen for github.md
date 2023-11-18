---
aliases: 
tags: 
description:
title: ssh-keygen for github
created: 2023-11-18T22:49:07
updated: 2023-11-18T23:00:09
---
- [[0010 Programming 👩‍💻|programming]]
- [connecting to github with ssh](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
---

## TL;DR

- 1단계: 키 생성

```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

- 2단계: 백그라운드에 ssh agent 등록

```shell
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/<private-key>
```

참고로, 저 위에 두 줄을 `$HOME/.bashrc` 파일에 넣으면 재부팅해도 자동으로 ssh agent에 등록된다.

- 3단계: 퍼블릭 키 등록

public key를 <https://github.com/settings/keys> 에 추가한다.

- 4단계: github.com에 ssh 연결

`$HOME/.ssh/config` 파일을 열어 다음을 추가한다.

```sshconfig
Host github.com
  HostName ssh.github.com
  User git
  PreferredAuthentications publickey
  IdentityFile "/path/to/private/key"
  AddKeysToAgent yes
```

다음 명령어를 실행하여 publickey 에러가 나지 않고 인삿말이 나오면 OK

```shell
ssh github.com
```

## 🆗

```
PTY allocation request failed on channel 0
Hi ChoiWheatley! You've successfully authenticated, but GitHub does not provide shell access.
Connection to ssh.github.com closed.
```

## 🙅‍♀️

```
git@ssh.github.com: Permission denied (publickey).
```
