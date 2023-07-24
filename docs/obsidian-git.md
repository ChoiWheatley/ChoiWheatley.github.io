---
aliases: 
tags: 
description:
title: obsidian-git
created: 2023-07-23T15:58:09
updated: 2023-07-24T20:17:06
---
- [obsidian-git](https://github.com/denolehov/obsidian-git)

## publickey - mac

- [Permission denied (publickey) {sof}](https://github.com/denolehov/obsidian-git/issues/42)

**해결법**

`$HOME/.ssh/config` 파일에 다음 내용을 추가한다.

```
Host github.com
    AddKeysToAgent yes
    User git
    IdentityFile "/Users/{{username}}/.ssh/{{private_key_name}}"
```

**원인**

obsidian-git 이 의존하는 [isomorphic-git](https://isomorphic-git.org/) 자체의 문제이다. HTTPS를 더 권장하는 이상한 정책을 가지고 있어 SSH에 상대적으로 덜 신경이 쏠린 탓일 수도 있다. isomorphic-git은 
