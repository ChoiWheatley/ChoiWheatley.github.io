---
description:
aliases: 
tags: 
created: 2023-05-02T11:04:06
updated: 2023-08-21T21:51:49
title: unittest module - python
---
- <https://docs.python.org/3/library/unittest.html>
- [timeout-decorator {pypi}](https://pypi.org/project/timeout-decorator/) -- unittest 모듈에 없는 타임아웃 기능을 제공합니다.
	 ```python
	import time
	import timeout_decorator
	
	@timeout_decorator.timeout(5)
	def mytest():
	    print("Start")
	    for i in range(1,10):
	        time.sleep(1)
	        print("{} seconds have passed".format(i))
	
	if __name__ == '__main__':
	    mytest()
	```
