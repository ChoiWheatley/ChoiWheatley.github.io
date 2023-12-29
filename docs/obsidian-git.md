---
aliases: 
tags: 
description:
title: obsidian-git
created: 2023-07-23T15:58:09
updated: 2023-08-20T02:38:17
---
- [obsidian-git](https://github.com/denolehov/obsidian-git)

## publickey - mac

- [Permission denied (publickey) {sof}](https://github.com/denolehov/obsidian-git/issues/42)

**해결법**

`ssh-add` 명령에 `--apple-use-keychain` 옵션을 추가한다. 이렇게 하면 passphrase를 유저에게 묻지 않고 애플 키체인을 통해 획득할 수 있다. (근데 나는 비밀번호 없이 생성했는데?)

예제

```shell
ssh-add --apple-use-keychain ~/.ssh/choi-workspace
```

**그래도 안된다면**

- [Authentication issue with ssh passphrase {GH issue}](https://github.com/denolehov/obsidian-git/issues/42#issuecomment-1150168064)

`~/.ssh/config` 파일 안에 다음 내용을 작성했는지 확인해보라.

```
Host github.com #custom name for the host
  HostName ssh.github.com #or other website directing to the host
  User git #usually your username, but github ssh takes git as the username
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa #add your ssh-key file path

AddKeysToAgent yes
```
