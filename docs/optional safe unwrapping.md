---
description:
aliases: 
tags: 
created: 2023-04-12T22:20:41
updated: 2023-07-11T15:21:07
title: optional safe unwrapping
---
```cpp
if (auto value = (*someSharedPtr)) {/*do something with `value`*/}
```