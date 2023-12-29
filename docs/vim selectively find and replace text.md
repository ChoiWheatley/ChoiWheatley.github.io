---
aliases: 
tags: 
description:
title: vim selectively find and replace text
created: 2023-11-04T06:48:24
updated: 2023-11-04T06:48:34
---
Yes, Vim provides an interactive way to perform search and replace. You can use the following command:

```
:%s/old_text/new_text/gc
```

This command initiates an interactive search and replace operation, where Vim will prompt you for each occurrence of “old_text” found in the file, allowing you to confirm whether to replace it with “new_text”. You can then choose to replace or skip each occurrence individually.
