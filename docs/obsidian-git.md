---
aliases: 
tags: 
description:
title: obsidian-git
created: 2023-07-23T15:58:09
updated: 2023-07-24T20:28:56
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
