---
description:
aliases: 
tags: 
created: 2023-05-31T14:39:00
updated: 2023-07-15T21:30:20
title: django template {context}
---
- [What is a context in django {SOF}](https://stackoverflow.com/questions/20957388/what-is-a-context-in-django#:~:text=A%20context%20is%20a%20variable%20name%20-%3E%20variable,specify%20the%20variables%20in%20each%20render%20%28%29%20call.)
- https://docs.djangoproject.com/en/4.2/ref/templates/api/#django.template.Context
- Context를 사용하면 장고 템플릿 파일에서 변수를 정의할 수 있다. 다만, `render`에게 해당 컨텍스트를 인수로 전달하여야 한다. | [[django render]]
	- 꼭 `render`에게 전달할 필요는 없고, `View`클래스가 `get_context_data`라는 메서드를 구현하여 내가 추가하고 싶은 변수를 dict 안에 추가하면 된다. | [[get_context_data, get_queryset { django } { ListView }]]

```python
lass DetailArticleView(DetailView):
    model = Article
    template_name = "blog/detail_article.html"

    def get_context_data(self, **kwargs: Any) -> Dict[str, Any]:
        """let's override this method to add more template context 'liked_by_user'"""
        context = super(DetailArticleView, self).get_context_data(**kwargs)
        article = Article.objects.get(id=self.kwargs.get("pk"))

        context["liked_by_user"] = False

        if article.likes.filter(pk=self.request.user.id).exists():
            context["liked_by_user"] = True

        return context
```

- `self.object`의 정체는 무엇인가? | [SingleObjectMixin {doc}](https://docs.djangoproject.com/en/4.2/ref/class-based-views/mixins-single-object/) 에서 가지고 있는 필드인데, 템플릿에서도 쓰이는 걸 보면 어딘가 중요한 놈인 것 같다. | 참고로 [MultipleObjectMixin {doc}](https://docs.djangoproject.com/en/4.2/ref/class-based-views/mixins-multiple-object/)에서는 `self.object_list`가 쓰인다.
- `self.object(_list)`와 `self.get_context_data()`와의 차이점을 잘 모르겠다...
