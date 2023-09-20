---
aliases: 
tags: 
description:
title: split {C}
created: 2023-09-20T17:10:31
updated: 2023-09-20T17:13:47
---
- [[0017 C 🍎]]
___

> simply divide buffer into two parts with given delimeter

```c
/// @brief split into two parts specified with delimeter
/// @param line [in]
/// @param left [out]
/// @param right [out]
/// @return 1 if success, 0 if failure, no delimeter found
int split(const char *line, char *left, size_t leftlen, char *right,
          size_t rightlen, const char delim) {
  char *pos = strchr(line, (int)delim);
  if (pos == NULL) {
    return 0;
  }

  // do copy left
  for (const char *itr = line; (itr - line) < leftlen && *itr != delim; ++itr) {
    size_t idx = itr - line;
    if (isspace(*itr)) continue;
    left[idx] = *itr;
  }

  // do copy right
  strncpy(right, pos + 1, rightlen);
  return 1;
}
```
