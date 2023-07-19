---
description:
aliases: 
tags: 
created: 2023-05-29T17:16:09
updated: 2023-07-19T15:36:05
title: get_context_data, get_queryset { django } { ListView }
---
- [docs](https://docs.djangoproject.com/en/4.1/ref/models/options/)
- [When to use `get`, `get_query_set`, `get_context_data` in Django? -- SOF](https://stackoverflow.com/questions/36950416/when-to-use-get-get-queryset-get-context-data-in-django)
---
- `get` 메서드를 오버라이딩하면 단순히 HTML 문서를 리턴한다. 이름답게 GET 메서드를 호출했을 때 내가 원하는 html 블럭을 리턴하도록.
- `get_context_data`는 HTML 문서가 아니라 `context`와 관련한 모든 `dict`를 리턴한다. 그래서 기존에 없던 속성을 추가해야 하는 경우 아주 유용하게 써먹을 수 있다. 예제에서는 `liked_by_user`라는 속성을 추가하여 리턴하는 저시기를 볼 수 있다. | [YT link](https://youtu.be/sMqDJovFO-Y?t=6682)
- `get_queryset`은 `ListView`에서 display하고싶은 오브젝트 리스트를 반환한다. 기본값으로는 필터링을 안한 URI에 대한 모든 오브젝트를 반환하기 때문에 주로 `filter`를 걸어서 원하는 저시기만 추출할 때 사용함.
	- 근데 [이 아저씨가 사용하는 모습](https://youtu.be/sMqDJovFO-Y?t=6815)을 보면 굳이 `get_queryset`을 사용하지 않고 그냥 바로 `filter`를 걸어버리는데, 무슨 차이를 낳는지 모르겠다.

# `get_queryset`

`ListView`에서 객체의 리스트를 리턴하는 메서드이다. 기본적으로는 `objects.all()`을 호출하지만 오버라이딩 하여 원하는 결과만을 필터링하거나 exclude 하여 리턴할 수 있다.

# `get_context_data`

> You will probably be overriding this method often to **add** things to display in your templates

주로 템플릿 컨텍스트(dict)에 원하는 엔트리를 추가하고자 할 때 사용한다.

```python
def get_context_data(self, **kwargs):
    data = super().get_context_data(**kwargs)
    data['page_title'] = 'Authors'
    return data
```

`pate_title`을 사용할 수 있게 되었다

```html
<h1>{{ page_title }}</h1>

<ul>
{% for author in author_list %}
    <li>{{ author.name }}</li>
{% endfor %}
</ul>
```
