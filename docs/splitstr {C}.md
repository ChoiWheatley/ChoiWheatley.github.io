---
aliases: 
tags: 
description:
title: splitstr {C}
created: 2023-09-20T23:09:08
updated: 2023-09-20T23:10:57
---
- [[0017 C 🍎]]
___

## description

delimeter 문자열을 기준으로 두 부분으로 나눈다. `delim`을 포함하지 않는다.

## source code

```c
/// @brief split into two parts specified wit delimeter
/// @param line [in]
/// @param left [out]
/// @param right [out]
/// @param delim [in] different with `split`, it takes string, not character
/// @return 1 if success, 0 if failure, no delimeter found
int splitstr(const char *line, char *left, size_t leftlen, char *right,
             size_t rightlen, const char *delim, size_t delimlen) {
  char *pos = strstr(line, delim);
  if (pos == NULL) {
    return 0;
  }

  // do copy left
  for (const char *itr = line; itr != pos; ++itr) {
    size_t idx = itr - line;
    if (isspace(*itr)) continue;
    left[idx] = *itr;
  }

  // do copy right
  strncpy(right, pos + delimlen, rightlen);
  return 1;
}
```
