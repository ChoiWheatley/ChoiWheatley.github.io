---
aliases: 
tags: 
description:
title: strcpy and strcat {C}
created: 2023-09-02T13:49:18
updated: 2023-09-02T13:51:48
---
<https://man7.org/linux/man-pages/man3/strcpy.3.html>

```
strcpy()
	  These functions copy the string pointed to by src, into a
	  string at the buffer pointed to by dst.  The programmer is
	  responsible for allocating a destination buffer large
	  enough, that is, strlen(src) + 1.  For the difference
	  between the two functions, see RETURN VALUE.

strcat()
	  This function catenates the string pointed to by src,
	  after the string pointed to by dst (overwriting its
	  terminating null byte).  The programmer is responsible for
	  allocating a destination buffer large enough, that is,
	  strlen(dst) + strlen(src) + 1.
```
