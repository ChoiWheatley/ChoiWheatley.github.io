---
aliases: 
tags: 
description:
title: trimwhitespace {C}
created: 2023-09-20T14:14:59
updated: 2023-09-20T14:421287:17
---
- <https://stackoverflow.com/a/122721/21369350> 참고함.
- [[0017 C 🍎]]
___
파이썬 `strip` 같은 함수가 필요했다. 그래서 검색해보니 `strip_left`는 단순히 리턴 포인터를 바꿔서 해결했고, `strip_right`는 뒤에서부터 순회하며 화이트스페이스가 아닌 위치 +1에 `\0`을 달아서 문제를 해결했다.

```c
// Note: This function returns a pointer to a substring of the original string.
// If the given string was allocated dynamically, the caller must not overwrite
// that pointer with the returned value, since the original pointer must be
// deallocated using the same allocator with which it was allocated.  The return
// value must NOT be deallocated using free() etc.
char *trimwhitespace(char *str)
{
  char *end;

  // Trim leading space
  while(isspace((unsigned char)*str)) str++;

  if(*str == 0)  // All spaces?
    return str;

  // Trim trailing space
  end = str + strlen(str) - 1;
  while(end > str && isspace((unsigned char)*end)) end--;

  // Write new null terminator character
  end[1] = '\0';

  return str;
}
```
