---
description:
aliases: 
tags: 
created: 2023-06-01T13:33:19
updated: 2023-07-20T06:38:43
title: django forms
---
- [Working with forms {doc}](https://docs.djangoproject.com/en/4.2/topics/forms/)
- [[form errors{ django }]]
- `django.forms`를 실제로 활용하고 있는 영상 | <https://youtu.be/sMqDJovFO-Y?t=1350>

# form, 너의 역할은 무엇이냐

- DRF만으로 데이터 전송할 것이 아니라면 무조건 들춰보게 될 유용한 도구이다. 접속자가 우리 웹에서 활동(글쓰기, 댓글 등)을 하는 여러 유즈케이스를 구현하는데 도움을 줄 것이다.
- 렌더링 이전에는 폼에 필요한 데이터들을 정렬한다.
- 정렬한 데이터를 가지고 HTML `<form>` 태그 안에 감싸 넣는다.
- 사용자 입력을 올바르게 가지고 와서 처리까지 수행한다.
- `<form>` 태그에 들어갈 context를 정의하는 객체이다.

# Form의 구성

## Media 

form과 연관된 js, css를 정의한다. [Form Assets (the Media class) {doc}](https://docs.djangoproject.com/en/4.2/topics/forms/media/)

## [`Widget`](https://docs.djangoproject.com/en/4.2/ref/forms/widgets/) 

필드 하나하나를 위젯이라고 부른다. form은 단순히 데이터이다. 템플릿 파일에서 필드를 어떻게 만들지는 순전히 템플릿 작성자의 몫이라는 것이다(물론 백/프론트 분리가 안돼 혼자서 해도 되겠지만). 구체적인 필드에 대한 HTML 코드조각을 백엔드서부터 정의하고자 바로 이 위젯이 나오게 되었다.

예를 들어[[tinymce]]는 그저 하나의 위젯을 제공해주는 도구에 불과하다.

# `ModelForms`

> It is _possible_ to write code that does all of this manually, but Django can take care of it all for you.

- [[django internal class Meta]]와의 훌륭한 합작으로 커스텀 Model을 폼으로 재창조하는 불편함 없이 `field`만 매핑하면 내부 작업을 알아서 수행해준다!!! | [django.forms.modelforms](https://docs.djangoproject.com/en/4.2/topics/forms/modelforms/)

```python
from django.forms import ModelForm
 myapp.models import Article

# Create the form class.
class ArticleForm(ModelForm):
     class Meta:
         model = Article
         fields = ["pub_date", "headline", "content", "reporter"]


# Creating a form to add an article.
form = ArticleForm()

# Creating a form to change an existing article.
article = Article.objects.get(pk=1)
form = ArticleForm(instance=article)
```

`fields`의 순서를 바꿨더니 실제 폼의 순서도 바뀌더라~~~

`fields = ["username", "email", "password1", "password2"]`  
![[Pasted image 20230601150945.png|350]] 

`fields = ["email", "username", "password1", "password2"]`  
![[Pasted image 20230601151020.png|350]]

# UserCreationForm

[[UserCreationForm {django}]]

# forms workflow

[Instantiating, processing, and rendering forms](https://docs.djangoproject.com/en/4.2/topics/forms/#instantiating-processing-and-rendering-forms)

1. `View`에서 `Form` 객체를 생성한다.
2. [[django template {context}]] template context에 해당 form을 넘겨준다.
3. [[django template]] 장고 템플릿 HTML 파일에서 넘겨받은 form을 그대로 붙여넣는다.

# plus alpha with crispy-forms

[[django crispy forms]]
