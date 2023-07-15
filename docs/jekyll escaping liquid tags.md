---
aliases: 
tags: 
description:
title: jekyll escaping liquid tags
created: 2023-07-12T10:17:57
updated: 2023-07-15T21:33:04
---
https://www.digitalocean.com/community/tutorials/jekyll-escaping-liquid-tags

가끔 liquid 문법을 옵시디언에 적어야 할 때가 있다. 이 문법은 꽤나 공통적이기 때문에 [[django reverse]]와 같이 대놓고 그 문법을 작성한 경우 jekyll build 시에 문법오류로 뻗게된다.

이 문제를 방지하고자 문법무시 기능이 있는데 그것이 바로 `{% raw %}`, `{% endraw %}` 쌍이다.

# 그런데,

그러면 매번 파일에 저 쌍을 파일에 추가하여야 한다는 것이 매우 성가시다. 따라서 기본적으로 `_notes` 폴더 안에 있는 파일들은 liquid 문법을 적용하지 않도록 설정을 바꿀 필요가 있었다. 

- https://talk.jekyllrb.com/t/any-way-to-exclude-files-directories-from-liquid-processing/6282

Jekyll도 frontmatter를 사용하여 메타데이터를 다룬다. 이때 `render_with_liquid: false`라는 엔트리를 추가하면 Jekyll 입장에서 liquid 대치를 안한다고 하니 아주 편해졌다.

- https://jekyllrb.com/docs/configuration/front-matter-defaults/

그리고 특정 폴더 안에 있는 모든 파일의 기본 frontmatter를 지정해 줄 수 있기 때문에, 해당 옵션을 전부 적용해주면 Jekyll이 알아서 무시한다.
