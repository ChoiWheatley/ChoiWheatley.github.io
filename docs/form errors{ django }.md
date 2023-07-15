---
aliases: 
tags: 
description:
created: 2023-07-06T09:10:31
updated: 2023-07-15T21:33:04
title: form errors{ django }
---

{% raw %}

- parent: [[django forms]]
- [docs - Rendering form error messages](https://docs.djangoproject.com/en/4.2/topics/forms/#rendering-form-error-messages)
- [docs - Form.errors](https://docs.djangoproject.com/en/4.2/ref/forms/api/#django.forms.Form.errors)
- `form.is_valid()`가 실패했을 때 기본적으로 추가되는 에러메시지 말고도 내가 에러를 폼에 넣어서 **자동으로** 렌더링 시킬 수 있다나 뭐라나.

# [Rendering fields manually](https://docs.djangoproject.com/en/4.2/topics/forms/#rendering-fields-manually) 

폼의 멤버를 참조할 수 있구만. `{{ form.name_of_field }}` 에러는 그 뒤에 `errors`를 붙이기만 하면 된다. `{{ form.name_of_field.errors }}` 또는 `{{ form.non_field_errors }}`

```python
{{ form.non_field_errors }}
<div class="fieldWrapper">
    {{ form.subject.errors }}
    <label for="{{ form.subject.id_for_label }}">Email subject:</label>
    {{ form.subject }}
</div>
<div class="fieldWrapper">
    {{ form.message.errors }}
    <label for="{{ form.message.id_for_label }}">Your message:</label>
    {{ form.message }}
</div>
<div class="fieldWrapper">
    {{ form.sender.errors }}
    <label for="{{ form.sender.id_for_label }}">Your email address:</label>
    {{ form.sender }}
</div>
<div class="fieldWrapper">
    {{ form.cc_myself.errors }}
    <label for="{{ form.cc_myself.id_for_label }}">CC yourself?</label>
    {{ form.cc_myself }}
</div>
```

# [Form.add_error()](https://docs.djangoproject.com/en/4.2/ref/forms/api/#django.forms.Form.add_error)

```python
Form.add_error(field, error)
```

구체적인 필드와 에러를 추가할 수 있다. 필드 자리에 `None`이 들어가면 자동으로 `non_field_errors` 안에 들어간다.


{% endraw %}
