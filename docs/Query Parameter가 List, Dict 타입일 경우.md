---
aliases: 
tags: 
description:
title: Query Parameter가 List, Dict 타입일 경우
created: 2024-12-05T16:42:32
updated: 2024-12-05T16:42:33
---
참고한 블로그: [https://lucyeun95.medium.com/how-to-set-get-a-list-type-query-param-in-django-rest-framework-831f30476111](https://lucyeun95.medium.com/how-to-set-get-a-list-type-query-param-in-django-rest-framework-831f30476111)

Django의 `QueryDict` 타입이 `getlist()` 메서드를 지원하며, 쿼리 파라메터의 리스트를 파싱해주는 역할을 수행합니다. (참고: [https://docs.djangoproject.com/en/4.2/ref/request-response/#querydict-objects](https://docs.djangoproject.com/en/4.2/ref/request-response/#querydict-objects)) 이때 jQuery.param과 Axios의 규격을 사용한다고 합니다. 따라서…

---

[https://api.jquery.com/jquery.param/](https://api.jquery.com/jquery.param/) 의 예시를 따르도록 제안합니다.

쿼리 파라메터가 리스트일 경우, `key[]` 문법을 사용하며, 다음과 같은 예시를 보여줍니다:

```jsx
var queryParam = {
	b: [1, 2, 3]
};

`
b[]=1&b[]=2&b[]=3
`
```

쿼리 파라메터가 Dict인 경우, `key[nest]` 문법을 사용하며 다음과 같은 예시를 보여줍니다:

```jsx
var myObject = {
  a: {
    one: 1,
    two: 2,
    three: 3
  },
  b: [ 1, 2, 3 ]
};
`
a[one]=1&a[two]=2&a[three]=3&b[]=1&b[]=2&b[]=3
`
```
