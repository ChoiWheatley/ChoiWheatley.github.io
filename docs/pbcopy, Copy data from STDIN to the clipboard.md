---
aliases: 
tags: 
description:
title: pbcopy, Copy data from STDIN to the clipboard
created: 2024-11-18T00:05:30
updated: 2024-11-18T00:08:03
---
- ref: <https://ss64.com/mac/pbcopy.html>
- 설치 ❌
---

The `pbcopy` command in macOS allows you to copy data from the standard input (STDIN) to the clipboard. By default, it uses the general pasteboard, but you can specify a different one with the `-pboard` option. For example, to copy the contents of a file to the clipboard, you can use:

```bash
cat filename.txt | pbcopy
```

This command reads the contents of `filename.txt` and copies them to the clipboard.  

## Examples

**Copy a list of files in your home directory to the macOS clipboard:**

> $ ls ~ | pbcopy

**Copy the contents of a file to the clipboard:**

> $ pbcopy < cookies.txt

**Copy part of a file to the clipboard:**

> $ grep 'ip address' serverlist.txt | pbcopy
