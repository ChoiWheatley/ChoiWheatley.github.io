---
aliases: 
tags:
  - scrap
description: 
title: scope of enum in C vs C++
created: 2024-01-18T22:14:23
updated: 2024-01-18T22:14:59
---
- [stackoverflow / Scope of enum in C VS C++](https://stackoverflow.com/questions/30047021/scope-of-enum-in-c-vs-c)

In C, there is simply no rule for scope for enums and struct. The place where you define your enum doesn't have any importance.

In C++, define something inside another something (like an enum in a class) make this something belong to the another something.

If you want to make your enum global in C++, you will have to define it outside your class, or access from your struct path:

```cpp
#include <iostream>
struct mystruct
{
    enum {INT, FLOAT, STRING} type;
    int integer;
    float floating_point;
} tu;

int main()
{
    tu.type = mystruct::INT; // INT is not in global scope, I have to precise it.
    tu.integer = 100;
    return 0;
}
```

**Note:** This works in this exemple, because you are using a `struct`, where everything is `public` by default. Be careful; you can access your enum type and values from outside your struct or your class only if the enum is in a `public` scope, as any field or function.
