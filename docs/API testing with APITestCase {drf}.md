---
aliases: 
tags: 
description:
title: API testing with APITestCase {drf}
created: 2024-12-05T00:39:23
updated: 2024-12-05T01:18:32
---
- ref: <https://www.django-rest-framework.org/api-guide/testing>

---

`APITestCase`는 Django REST Framework(DRF)에서 제공하는 테스트 도구로, API의 동작을 확인하고 검증하기 위한 강력한 기능을 제공합니다. `APITestCase`는 Django의 표준 `TestCase`를 확장하며, 클라이언트 객체를 사용해 HTTP 요청을 테스트할 수 있습니다.

---

## 1. **APITestCase의 주요 기능**

- **HTTP 요청 테스트**: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`와 같은 HTTP 메서드를 쉽게 테스트할 수 있습니다.
- **RESTful API 테스트**: REST API의 상태 코드, 응답 데이터, 헤더 등을 검증할 수 있습니다.
- **인증 테스트**: 인증된 사용자와 비인증 사용자를 구분하여 API 접근 권한을 테스트할 수 있습니다.
- **Database Rollback**: 각 테스트마다 데이터베이스 변경사항이 롤백되어 테스트 환경을 깨끗하게 유지합니다.

---

## 2. **APITestCase 사용 방법**

`APITestCase`를 사용하려면 `rest_framework.test` 모듈에서 가져와야 합니다.

```python
from rest_framework.test import APITestCase
from rest_framework import status
from django.urls import reverse
```

---

### 2.1. **예제: 간단한 CRUD API 테스트**

```python
from rest_framework.test import APITestCase
from rest_framework import status
from django.urls import reverse
from myapp.models import Article

class ArticleAPITestCase(APITestCase):
    def setUp(self):
        # 테스트 데이터 생성
        self.article_data = {'title': 'Test Title', 'content': 'Test Content'}
        self.article = Article.objects.create(**self.article_data)
        self.article_url = reverse('article-detail', args=[self.article.id])

    def test_get_article_list(self):
        # GET 요청 테스트
        url = reverse('article-list')
        response = self.client.get(url)

        # 응답 검증
        self.assertEqual(response.status_code, status.HTTP_200_OK)
        self.assertIn('title', response.data[0])

    def test_create_article(self):
        # POST 요청 테스트
        url = reverse('article-list')
        data = {'title': 'New Title', 'content': 'New Content'}
        response = self.client.post(url, data)

        # 응답 검증
        self.assertEqual(response.status_code, status.HTTP_201_CREATED)
        self.assertEqual(response.data['title'], 'New Title')

    def test_update_article(self):
        # PUT 요청 테스트
        data = {'title': 'Updated Title', 'content': 'Updated Content'}
        response = self.client.put(self.article_url, data)

        # 응답 검증
        self.assertEqual(response.status_code, status.HTTP_200_OK)
        self.assertEqual(response.data['title'], 'Updated Title')

    def test_delete_article(self):
        # DELETE 요청 테스트
        response = self.client.delete(self.article_url)

        # 응답 검증
        self.assertEqual(response.status_code, status.HTTP_204_NO_CONTENT)
        self.assertFalse(Article.objects.filter(id=self.article.id).exists())
```

---

## 3. **인증 및 권한 테스트**

`APITestCase`를 사용하여 인증 및 권한 관련 테스트를 수행할 수 있습니다.

### 3.1. **Token Authentication 테스트**

```python
from rest_framework.test import APITestCase
from rest_framework.authtoken.models import Token
from django.contrib.auth.models import User

class AuthenticationAPITestCase(APITestCase):
    def setUp(self):
        # 사용자 생성 및 토큰 발급
        self.user = User.objects.create_user(username='testuser', password='testpassword')
        self.token = Token.objects.create(user=self.user)
        self.client.credentials(HTTP_AUTHORIZATION=f'Token {self.token.key}')

    def test_authenticated_request(self):
        url = reverse('protected-resource')
        response = self.client.get(url)

        self.assertEqual(response.status_code, status.HTTP_200_OK)
```

---

## 4. **APITestCase의 주요 메서드**

- `self.client.get(url, data=None, format=None)`  
  GET 요청을 보냅니다.

- `self.client.post(url, data, format=None)`  
  POST 요청을 보냅니다.

- `self.client.put(url, data, format=None)`  
  PUT 요청을 보냅니다.

- `self.client.patch(url, data, format=None)`  
  PATCH 요청을 보냅니다.

- `self.client.delete(url, data=None, format=None)`  
  DELETE 요청을 보냅니다.

- `self.client.credentials(**kwargs)`  
  헤더를 설정합니다. 인증 토큰을 포함할 때 유용합니다.

- `self.assertEqual(a, b)`  
  값이 같은지 확인합니다.

- `self.assertIn(member, container)`  
  요소가 포함되어 있는지 확인합니다.

---

## 5. **장점**

- Django의 기본 테스트 환경을 활용하면서 REST API 테스트에 특화된 기능을 제공합니다.
- 데이터베이스 변경사항이 테스트 후 롤백되어 다른 테스트에 영향을 주지 않습니다.
- DRF의 인증 및 권한 체계를 쉽게 검증할 수 있습니다.

---

## 6. **테스트 실행**

`manage.py` 명령어로 테스트를 실행할 수 있습니다.

```bash
python manage.py test
```

---

`APITestCase`는 REST API의 다양한 기능을 테스트하는 데 필요한 강력한 도구를 제공합니다. 이를 통해 API의 안정성과 품질을 확보할 수 있습니다.
