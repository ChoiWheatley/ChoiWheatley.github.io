---
aliases: 
tags: 
description:
title: UpdateView {django}
created: 2023-07-25T16:07:06
updated: 2023-07-25T18:03:09
---
[[CCBV, Classy Class Based View -- django]]에서 참고 많이했다. 글 수정을 위한 뷰 클래스 작성을 하는데 `get`, `post` 메서드를 만들지 않고도 가능하다니

그것은 애트리뷰트만 잘 알고 있으면 된다. 특히나 CCBV를 참고하면 복잡한 상속관계에서 오는 메서드, 애트리뷰트를 한 눈에 볼 수 있다. 거기에서 내가 원하는 확장만 추가하면 된다.

어떤 믹스인은 반드시 정의해야 하는 애트리뷰트가 있다. 그걸 정의하지 않으면 `ImproperlyConfigured` 예외가 발생할 수 있으니 문제가 생기더라도 놀라지 말자.

## Attributes

- `model`
- `template_name`
- `form_class`
- `success_url`
- `queryset`

## Methods

- `get_form_kwargs`: keyword argument를 확장할 수 있는 메서드이다.
	- [How do I Use CreateView with a ModelForm {SOF}](https://stackoverflow.com/a/5774124/21369350)
	- [Editing mixins {doc}](https://docs.djangoproject.com/en/4.2/ref/class-based-views/mixins-editing/#django.views.generic.edit.FormMixin.get_form_kwargs)

- `form_valid`
	- [class-based-views {doc}](https://docs.djangoproject.com/en/4.2/topics/class-based-views/generic-editing/)
	- [Models and `request.user` {doc}](https://docs.djangoproject.com/en/4.2/topics/class-based-views/generic-editing/#models-and-request-user)
	- `django.views.generic.ModelFormMixin.form_valid()`를 따라가보면 `form_valid` 안에 `form.save()`를 호출하는 것을 볼 수 있다. 따라서 우리가 `post` 메서드를 직접 구현할 때 form에 없던 `author`를 추가했었던 걸 기억해보자. 객체지향 안에서 기능을 확장하거나 후크를 걸기 위해선 오버라이드를 하여야 한다. 
	- `form.instance.<attr>` 를 통하여 form을 확장할 수 있다.

	```python
	from django.contrib.auth.mixins import LoginRequiredMixin
	from django.views.generic.edit import CreateView
	from myapp.models import Author
	
	
	class AuthorCreateView(LoginRequiredMixin, CreateView):
	    model = Author
	    fields = ["name"]
	
	    def form_valid(self, form):
	        form.instance.created_by = self.request.user
	        return super().form_valid(form)	
	```
