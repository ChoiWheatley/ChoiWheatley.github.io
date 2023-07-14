---
aliases: 
tags: 
description:
created: 2023-06-22T22:01:05
updated: 2023-07-11T15:21:09
title: clear cache in javascript
---
- parent: [[Javascript]]
- [sof](https://stackoverflow.com/questions/1011605/clear-the-cache-in-javascript)
- 장고 프로젝트를 하다가 보니까 따로 빼놓은 js 파일을 수정해도 금방 적용이 되지 않는 것을 알 수 있다. 위의 대화를 살펴보니  [`window.location.reload(true)`](http://msdn.microsoft.com/en-us/library/ms536691%28VS.85%29.aspx) 를 수행하면 묻고 따지지도 않고 캐시를 초기화 하는 것을 알 수 있다.
- [그냥 Ctrl + Shift + R](https://superuser.com/questions/1190499/how-to-perform-a-hard-refresh-in-edge)