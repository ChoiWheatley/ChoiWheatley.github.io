---
aliases: 
tags: 
description:
created: 2023-06-25T12:13:41
updated: 2023-07-15T21:33:03
title: reverse를 사용해야 하는 이유 {django}
---
- [Reverse resolution of URLs {docs.com}](https://docs.djangoproject.com/en/4.2/topics/http/urls/#reverse-resolution-of-urls)
- [Don't Repeat Yourself (DRY principle)](https://www.webforefront.com/django/designprinciples.html)
- [Reverse resolution of URLs {docs.com}](https://docs.djangoproject.com/en/4.2/topics/http/urls/#reverse-resolution-of-urls)

# TL;DR

- reverse는 URL 하드코딩을 피할 수 있는 좋은 도구이다.
- DRY 메커니즘을 사용하면 URL을 교체하는 것과 업무로직을 분리할 수 있다.

# 번역

Django 프로젝트에서 작업할 때 일반적으로 필요한 것은 생성된 콘텐츠(뷰 및 자산 URL, 사용자에게 표시되는 URL 등)에 임베드하거나 서버 측의 탐색 흐름(리디렉션 등)을 처리하기 위해 최종 형식의 URL을 가져오는 것입니다.

이러한 URL을 하드코딩하는 것은 피하는 것이 좋습니다(힘들고 확장성이 떨어지며 오류가 발생하기 쉬운 전략). URLconf에서 설명하는 설계와 평행한 URL을 생성하기 위해 임시 메커니즘을 고안하는 것도 마찬가지로 위험하며, 시간이 지남에 따라 부실해지는 URL을 생성할 수 있습니다.

즉, 필요한 것은 DRY 메커니즘입니다. 이 메커니즘을 사용하면 프로젝트 소스 코드를 모두 검토하여 오래된 URL을 검색하고 교체할 필요 없이 URL 디자인을 발전시킬 수 있다는 장점이 있습니다.

URL을 가져오는 데 사용할 수 있는 주요 정보는 해당 URL을 처리하는 뷰의 식별 정보(예: 이름)입니다. 올바른 URL을 조회하기 위해 반드시 참여해야 하는 다른 정보로는 뷰 인수의 유형(위치, 키워드)과 값이 있습니다.

장고는 URL 매퍼가 URL 디자인의 유일한 저장소가 되도록 솔루션을 제공합니다. URLconf와 함께 제공하면 양방향으로 사용할 수 있습니다:

사용자/브라우저가 요청한 URL부터 시작하여 URL에서 추출한 값과 함께 필요한 인수를 제공하는 올바른 Django 뷰를 호출합니다.  
해당 Django 뷰의 식별과 함께 해당 뷰에 전달될 인수의 값부터 시작하여 연결된 URL을 가져옵니다.  
첫 번째는 이전 섹션에서 논의했던 사용 방법입니다. 두 번째는 URL의 역방향 확인, 역방향 URL 일치, 역방향 URL 조회 또는 단순히 URL 반전이라고 알려진 것입니다.

Django는 URL이 필요한 여러 계층과 일치하는 URL 반전을 수행하기 위한 도구를 제공합니다:

템플릿에서: URL 템플릿 태그 사용.  
Python 코드에서: reverse() 함수 사용.  
Django 모델 인스턴스의 URL 처리와 관련된 상위 수준 코드에서: get_absolute_url() 메서드.
