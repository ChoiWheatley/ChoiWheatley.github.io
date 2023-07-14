---
description:
aliases: 
tags: 
created: 2023-06-07T11:13:59
updated: 2023-07-11T15:21:07
title: "`User`를 커스텀 하고싶으면, `User`말고도 다른 부분의 코드도 함께 변경해야 한다"
---
# TL;DR
default `User`를 의존하는 코드
- forms.py
	- 커스텀 폼의 `Meta`를 만들어 내가 넣고 싶은 필드만 `fields`에 넣는다. 이건 쉽지.
- admin.py
	- `django.contrib.auth.UserAdmin` 소스코드를 잘 읽어보면 `form`, `add_form`이라는 멤버가 있 다. 디폴트로 각각 `UserChangeForm`과 `UserCreationForm`을 composite 패턴 형태로 가지고 있는데, 여기에서 내가 오버라이딩한 `username`을 참조하는 것이었다. 따라서 이 멤버들을 오버라이드 하여야 한다.

# Question
> `UserCreationForm` requires `username` field. But I already override `username` with `email`. How can I resolve this error? Error message:

```
Watching for file changes with StatReloader
Exception in thread django-main-thread:
Traceback (most recent call last):
  File "/home/linuxbrew/.linuxbrew/opt/python@3.11/lib/python3.11/threading.py", line 1038, in _bootstrap_inner
    self.run()
  File "/home/linuxbrew/.linuxbrew/opt/python@3.11/lib/python3.11/threading.py", line 975, in run
    self._target(*self._args, **self._kwargs)
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/utils/autoreload.py", line 64, in wrapper
    fn(*args, **kwargs)
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/core/management/commands/runserver.py", line 125, in inner_run
    autoreload.raise_last_exception()
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/utils/autoreload.py", line 87, in raise_last_exception
    raise _exception[1]
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/core/management/__init__.py", line 394, in execute
    autoreload.check_errors(django.setup)()
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/utils/autoreload.py", line 64, in wrapper
    fn(*args, **kwargs)
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/__init__.py", line 24, in setup
    apps.populate(settings.INSTALLED_APPS)
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/apps/registry.py", line 124, in populate
    app_config.ready()
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/contrib/admin/apps.py", line 27, in ready
    self.module.autodiscover()
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/contrib/admin/__init__.py", line 50, in autodiscover
    autodiscover_modules("admin", register_to=site)
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/utils/module_loading.py", line 58, in autodiscover_modules
    import_module("%s.%s" % (app_config.name, module_to_search))
  File "/home/linuxbrew/.linuxbrew/opt/python@3.11/lib/python3.11/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<frozen importlib._bootstrap>", line 1206, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1178, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1149, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 690, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 940, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/home/chltm/workspace/bookstore/users/admin.py", line 5, in <module>
    from .forms import SignupForm
  File "/home/chltm/workspace/bookstore/users/forms.py", line 6, in <module>
    class CustomUserCreationForm(UserCreationForm):
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/forms/models.py", line 321, in __new__
    raise FieldError(message)
django.core.exceptions.FieldError: Unknown field(s) (username) specified for User
```

# Answer
To resolve the error, you need to update the `UserCreationForm` to use the `email` field instead of the `username` field. Follow these steps:

1. Create a custom form, `CustomUserCreationForm`, that inherits from `UserCreationForm`.

```python
from django.contrib.auth.forms import UserCreationForm
from .models import User

class CustomUserCreationForm(UserCreationForm):
    class Meta(UserCreationForm.Meta):
        model = User
        fields = ('email',)
```

2. In your `admin.py` file, import and use the `CustomUserCreationForm` in the `add_form` field of the `UserAdmin` class.

```python
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin as BaseUserAdmin
from .models import User
from .forms import CustomUserCreationForm

class UserAdmin(BaseUserAdmin):
    add_form = CustomUserCreationForm

admin.site.register(User, UserAdmin)
```

By doing this, you're specifying the `CustomUserCreationForm` as the form to be used when adding a user in the Django admin. This form only includes the `email` field, allowing you to create a user without the `username` field while still maintaining the uniqueness constraint on the `username` field.