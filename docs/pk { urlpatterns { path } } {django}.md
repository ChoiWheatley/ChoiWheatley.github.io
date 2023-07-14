---
description:
aliases: 
tags: 
created: 2023-05-29T16:21:43
updated: 2023-07-12T10:41:32
title: pk { urlpatterns { path } } {django}
---

{% raw %}

- [What is difference between `<int:pk>` and `<pk>` {SOF}](https://stackoverflow.com/questions/62804254/what-is-the-difference-between-intpk-and-pk)
	- `<pk>` 보다는 `<int:pk>`를 선호하는 편이 좋다. [[django urlpatterns and url convert]]에 따르면 타입을 붙이면 언제나 string으로 들어오는 URL 요청으로부터 regex를 사용하여 파싱을 하게 된다고 한다. 그 결과, 구체적인 url patterns케이스를 작성할 수 있으며, 심지어 같은 path의 계층 일부에 타입을 부과하면 장고가 혼동할 여지를 줄일 수 있다.
- [Build a blog using django {YT}](https://youtu.be/sMqDJovFO-Y?t=5323)
	- `url_patterns` > [[django path(route, view, name)|path]] 에 대하여 라우트를 줄 때 `<int:pk>`를 줄 수 있다고 했다. 
	- 그말은 곧 인자를 하나 더 받겠다는 의미이다. 따라서 장고 템플릿에서 `{% url <name> [args] %}`의 규칙에 따라 인자를 추가하여야 한다. `pk`에는 `object.id`인듯?
	```python
	# blog > urls.py
	urlpatterns = [
	    path("<int:pk>/", DetailArticleView.as_view(), name="detail_article"),
		...
	]

	# blog > view.py
	class DetailArticleView(DetailView):
	    model = Article
	    template_name = "blog/detail_article.html"
	```
 
	```html
	<!-- blog > templates > blog > index.html -->
	{% for object in object_list %}
		...
		<a href="{% url 'detail_article' object.id %}" class="btn btn-primary">Read More</a>
	{% endfor %}
	```
 
{% endraw %}