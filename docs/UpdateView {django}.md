---
aliases: 
tags: 
description:
title: UpdateView {django}
created: 2023-07-25T16:07:06
updated: 2023-07-25T17:30:52
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
	- 예를 들어 `class New(CreateView)`를 작성할때 정의한 form에는 굳이 `author`가 있을 필요가 없다. 이미 `request`안에 유저 정보가 들어있기 때문이다. 하지만 그렇다고 form에 저거를 넣지 않으면 `author_id`가 없다고 `IntegrityError` 예외가 날아온다. 
	- 그래서 `get_form_kwargs`가 디폴트로 `author`를 채워주는 역할을 한다. 다음 코드예제를 보자.

	```python
	class New(LoginRequiredMixin, CreateView):
	    """Create new article"""
	
	    model = Article
	    template_name = "write.html"
	    form_class = ArticleForm
	    success_url = reverse_lazy("articles:list")
	    queryset = model.objects.all()
	
	    def get_form_kwargs(self) -> Dict[str, Any]:
	        kwargs = super().get_form_kwargs()
	        kwargs['author'] = self.request.user
	        return kwargs
	```
