---
aliases: 
tags: 
description:
created: 2023-06-23T10:01:40
updated: 2023-07-15T21:33:05
title: Django was not configured. For more information run pylint --load-plugins=pylint_django --help-msg=django-not-configured
---
- [해결법 {SOF}](https://stackoverflow.com/questions/65761250/pylint-django-raising-error-about-django-not-being-configured-when-thats-not-th)

Add the `--django-settings-module` argument the VS Code settings:

```json
  {
    "python.linting.pylintArgs": [
        [...]
        "--django-settings-module=<mainapp>.settings",
    ]
  }
```

Change `<mainapp>` to be your main app directory. For example, if your `settings.py` was in `sweetstuff/settings.py` then the argument value would be `sweetstuff.settings`. This is the same format as you would import the settings module from inside a Python module or the Django shell.
