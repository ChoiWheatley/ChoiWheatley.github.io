---
description:
aliases: 
tags: 
created: 2023-05-29T17:54:31
updated: 2023-07-20T07:25:30
title: tinymce
---
[official link](https://www.tiny.cloud/tinymce/)  
[django-tinymce doc](https://django-tinymce.readthedocs.io/en/latest/)
___
- wysiwyg rich-text embeddable editor
- [tinymce 사용하는 YT Link](https://youtu.be/sMqDJovFO-Y?t=7351)

## how to use it *inside* the admin site?

admin site에서는 자동으로 `django.contrib.admin` 템플릿이 얘를 렌더해주기 때문에 모델 필드에 딱 한 줄만 적으면 됐었다.

```python
class Article(models.Model):
	...
    content = tinymce.models.HTMLField()
	...
```

![[Pasted image 20230720084804.png]]

## how to use it *outside* the admin site?

문서가 오지게 불친절하고, 심지어 쓸데도 없는 `FlatPage`에 의존하는 코드를 넣어놔서 한참을 헤맸다. 그래도 한 마디로 정리하자면,

> [!NOTE]  
> `django-tinymce`모듈은 미리 정의된 Form **Widget**이다

필요한 작업만 쏙쏙 뽑아서 정리해보자.

```shell
pip install django-tinymce
```

in `settings.py`

```python
INSTALLED_APPS = [
	...
	"tinymce",
	...
]
...

# cdn
TINYMCE_JS_URL = 'https://cdn.tiny.cloud/1/no-api-key/tinymce/5/tinymce.min.js'
# installation
TINYMCE_JS_URL = os.path.join(STATIC_URL, "path/to/tiny_mce/tiny_mce.js")

```

in `core/urls.py`

```python
urlpatterns = patterns[
	...
	path("tinymce/", include("tinymce.urls")),
	...
]
```

in template file which depends on tinymce form

```html
<head>
	...
	{{ form.media }}
</head>
```

in `forms.py`

그러니까, 우리가 평소 쓰는 Form과 똑같은데, tinymce를 쓰는 필드에만 `widget`을 붙인다고 보면 된다.

```python
class NewArticleForm(forms.ModelForm):
    """TinyMCE widget"""

    class Meta:
        model = Article
        widgets = {"content": TinyMCE(attrs={"cols": 80, "rows": 30})}
        fields = ["title", "content", "category"]
```
