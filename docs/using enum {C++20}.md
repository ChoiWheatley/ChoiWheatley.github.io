---
aliases: 
tags: 
description:
title: using enum {C++20}
created: 2024-01-18T22:16:40
updated: 2024-01-18T22:17:07
---
- [stackoverflow / How to write a using statement for enum classes?](https://stackoverflow.com/questions/16974475/how-to-write-a-using-statement-for-enum-classes)

It will be possible in C++20, [P1099R5](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1099r5.html) :

```cpp
enum class choice {rock, paper, scissors};

using enum choice;
```
