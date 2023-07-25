---
aliases: 
tags: 
description:
title: UpdateView {django}
created: 2023-07-25T16:07:06
updated: 2023-07-25T16:21:13
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
