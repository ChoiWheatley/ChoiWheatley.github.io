---
aliases: 
tags: 
description:
title: strtok, strtok_r(3)
created: 2023-10-05T08:09:03
updated: 2023-10-05T09:00:08
---
- [[0017 C 🍎]]
- [한글 블로그](https://www.it-note.kr/86)
- [man page](https://man7.org/linux/man-pages/man3/strtok.3.html)
___

## synopsis

```c
       #include <string.h>

       char *strtok(char *restrict str, const char *restrict delim);
       char *strtok_r(char *restrict str, const char *restrict delim,
                      char **restrict saveptr);
```

`strtok_r`은 thread-safe한 버전의 `strtok`이다. 전역변수를 사용하지 않고 `saveptr`이라는 외부변수를 사용하기 때문.

## example

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int
main(int argc, char *argv[])
{
   char *str1, *str2, *token, *subtoken;
   char *saveptr1, *saveptr2;
   int j;

   if (argc != 4) {
	   fprintf(stderr, "Usage: %s string delim subdelim\n",
			   argv[0]);
	   exit(EXIT_FAILURE);
   }

   for (j = 1, str1 = argv[1]; ; j++, str1 = NULL) {
	   token = strtok_r(str1, argv[2], &saveptr1);
	   if (token == NULL)
		   break;
	   printf("%d: %s\n", j, token);

	   for (str2 = token; ; str2 = NULL) {
		   subtoken = strtok_r(str2, argv[3], &saveptr2);
		   if (subtoken == NULL)
			   break;
		   printf("\t --> %s\n", subtoken);
	   }
   }

   exit(EXIT_SUCCESS);
}
```

**result**

```shell
$ ./a.out 'a/bbb///cc;xxx:yyy:' ':;' '/'
1: a/bbb///cc
		--> a
		--> bbb
		--> cc
2: xxx
		--> xxx
3: yyy
		--> yyy
```

**해석**

위의 예제는 이중 for문을 활용하여 nested token 분리를 수행하는 방법을 나타내고 있다.

delim은 파이썬 식의 delimiter와는 조금 다르다. 개별적인 char 바이트 중 하나라도 일치하는 것이 있다면 통과하기 때문이다. 예를 들어 위의 사용예제에서 첫번째 delim을 `:;`으로 주었는데 `:` 또는 `;`에 대하여 모두 분리를 수행했다. 

`strtok`는 토큰화할 문자열이 더 이상 남아있지 않는 경우에는 NULL을 리턴하고 그 외에는 비어있지 않은 문자열의 첫번째 주소를 리턴한다. 모든 delim들은 Null-terminated character로 **수정**된다는 점 유의.

반복문에서 볼 수 있다시피 같은 함수를 반복하여 호출하지만 `saveptr` 안에 현재 위치 정보를 가지고 있기 때문에 괜찮다. 공식문서에서는 처음 호출에만 첫번째 인자 `str`이 필요하고 두 번째 호출서부터는 NULL이어야 한다고는 하지만 예제코드에서 버젓이 for문 안에 그냥 넣은 걸 보면 `saveptr`가 set 된 이후부터는 무시하는걸지도 모르겠다. 구현에 따라 첫 호출에는 `saveptr`를 NULL로 두는 경우가 있다고도 하는데 구체적인 얘기가 없는 걸 보면 무시해도 좋을지도.
