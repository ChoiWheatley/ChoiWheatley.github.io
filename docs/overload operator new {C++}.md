---
aliases: 
tags: 
description:
title: overload operator new {C++}
created: 2024-01-13T16:58:45
updated: 2024-01-13T17:00:58
---
- [[string {C++}]]
- <https://modoocode.com/292#page-heading-2>

Short String Optimization을 수행할때 예제 코드가 인상깊어서 아카이브함. `new ClassName(parameters)` 문법은 많이 써봤으나 이것을 다시 정의할 수 있다는 것을 알았다. 

```cpp
void *operator new(size_t count) {
	cout << count << "bytes 할당" << endl;
	return malloc(count);
}
```
