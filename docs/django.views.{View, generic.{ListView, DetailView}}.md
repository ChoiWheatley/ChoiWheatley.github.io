---
description:
aliases: 
tags: 
created: 2023-05-27T16:53:07
updated: 2023-07-15T21:33:05
title: django.views.{View, generic.{ListView, DetailView}}
---


{% raw %}

# View

https://docs.djangoproject.com/en/4.2/ref/class-based-views/base/#view

- attr
	- `http_method_names`: `['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']
- methods
	- *classmethod* `as_view`: HttpRequest를 호출하는 함수 (`get`, `post`, etc.)를 매칭한다.

`urls.py`에서 내가 리스닝을 할 요청과 그에 대한 콜백함수를 호출하는데 사용. 

```python
urlpatterns = [
    path("mine/", MyView.as_view(), name="my-view"),
]
```

# generic.ListView

https://docs.djangoproject.com/en/4.2/ref/class-based-views/generic-display/#listview

객체의 리스트를 표현하는 제네릭 뷰. 따라서 [[django template]]에서 순회를 돌 수 있다.

> While this view is executing, `self.object_list` will contain the list of objects (usually, but not necessarily a queryset) that the view is operating upon.

```html
        {%for object in object_list%}
        <div class="row">
            <div class="mx-md-auto mt-5 mb-5">
                <h3 class="fw-bold">{{object.title}}</h3>
                <p class="text-muted">{{object.date}}</p>
                <p class="text-muted">Posted by {{object.author.username}}</p>
                {%if object.likes.count == 1%}
                    <div class="text-muted">{{object.likes.count}} person likes this post</div>
                {%else%}
                    <div class="text-muted">{{object.likes.count}} people likes this post</div>
                {%endif%}
            </div>
        </div>
        {%endfor%}
```

`{% for object in object_list %}` 의 `object_list`가 서로 연결된다!

# generic.DetailView

[DetailView 작성하는 영상](https://youtu.be/sMqDJovFO-Y?t=4993)  
[djangoproject.com -- class based views -- generic display](https://docs.djangoproject.com/en/4.2/ref/class-based-views/generic-display/)

예를 들어 아티클을 보여주고자 할때 어떤 아티클을 보여줄지는 우리의 재량에 달렸다. 이 경우, `urls.py`에 `urlpatterns` 리스트에 `path`를 지정해줄때 `<slug:slug>/` 식의 URI를 제공하여야 한다. 

uri예시: `posts/<int:pk>/` 

자세한 활용예시는 위의 링크 따라가서 보세요.

템플릿에서 `{% url 'detail_article' %}` 이라고만 주었을 때 추가 인자가 없다고 징징댄다. 이때 내가 정확히 선택한 아티클의 id를(pk) 어떻게 받아올까? ==> 그냥 `object.id`를 'detail_article' 뒤에 추가해버리니까 되네???? 😲



[[slug가 뭐냐]]

# 파라미터에 있던 HttpRequest

https://docs.djangoproject.com/en/4.2/ref/request-response/#django.http.HttpRequest

- scheme
- body
- path
- path_info
- method
- encoding
- content_type
- content_params
- headers
- dictionary를 들고있음
	- GET
	- POST
 
{% endraw %}
