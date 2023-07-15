---
aliases: 
tags: 
description:
created: 2023-07-01T23:40:57
updated: 2023-07-15T21:30:21
title: aws s3 static files in django
---
- [testdriven.io {tutorial}](https://testdriven.io/blog/storing-django-static-and-media-files-on-amazon-s3/)
- [CORS 구성 - Amazon Simple Storage Service {amz}](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/ManageCorsUsing.html)
- [🌐 악명 높은 CORS 개념 & 해결법 - 정리 끝판왕 👏 {inpa}](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F)
- [[HTTP header CORS policy - 'Access-Control-Allow-Origin']]
- [Docker 설정 {ian}](https://velog.io/@iankimdev/Docker-%EC%84%A4%EC%A0%95)
- [S3 {ian}](https://velog.io/@iankimdev/S3)
- [EC2 {ian}](https://velog.io/@iankimdev/EC2)
- [S3 Bucket Policy {amz}](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/access-policy-language-overview.html?icmpid=docs_amazons3_console)
- [Amazon S3 {django-storages}](https://django-storages.readthedocs.io/en/latest/backends/amazon-S3.html?highlight=STATICFILES_STORAGE)
- [How to get Django to make some files public and media files private on AWS S3 (no 403 errors)?](https://stackoverflow.com/questions/67650507/how-to-get-django-to-make-some-files-public-and-media-files-private-on-aws-s3-n)
- [botocore.exceptions.ClientError: HeadObject operation: Not Found](https://stackoverflow.com/questions/44895334/botocore-exceptions-clienterror-an-error-occurred-404-when-calling-the-headob)
---
- 루트 사용자 말고 IAM 사용자를 하나 만들어서 루트에 연결해야함. 
- 루트계정에 MFA 인증을 추가한다.
- IAM 계정으로 로그인 후 인증키를 발급받는다. WHY? 보안 자격증명을 만들어주기 위해
- 버킷
	- 정적 웹 사이트 호스팅 할 때 필요한 `index.html` 파일을 작성하여 업로드 및 활성화
	- 버킷 정책 설정 [docs.com](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/access-policy-language-overview.html) 
		- 어떤 유저에게 어떤 리소스에 대한 어떤 접근을 허용/차단할지 등에 대한 정책임.
	- ACL (Access Control List) 소유권 활성화 (안해도 돼)
 

# 장고 프로젝트로 돌아와서...

- 의존성 설치하고 settings.py 수정하고, core/env.py 설정하고 
- settings.py 전역상수 설정 (book-project는 별도의 파일을 빼냈지만 근본은 동일.)

 ```python
 from core.env import config

AWS_ACCESS_KEY_ID=config("AWS_ACCESS_KEY_ID", default=None)
AWS_SECRET_ACCESS_KEY=config("AWS_SECRET_ACCESS_KEY", default=None)
AWS_S3_ADDRESSING_STYLE = "virtual"

AWS_STORAGE_BUCKET_NAME=config("AWS_STORAGE_BUCKET_NAME", default="estsoft-ormi-bookstore")
AWS_S3_REGION_NAME="ap-northeast-2"
AWS_S3_URL = f"https://{AWS_STORAGE_BUCKET_NAME}.s3.amazonaws.com"

AWS_DEFAULT_ACL="public-read"
AWS_S3_USE_SSL=True


DEFAULT_FILE_STORAGE = 'core.storages.backends.MediaStorage'
STATICFILES_STORAGE = 'core.storages.backends.StaticFileStorage'
 ```

 - `DEFAULT_FILE_STORAGE`, `STATICFILES_STORAGE` 를 기본 클래스를 확장한 커스텀 클래스인 `MediaStorage, StaticFileStorage`로 변경한 것을 볼 수 있다. 파일을 들여다보자.

```python
from storages.backends.s3boto3 import S3Boto3Storage, S3StaticStorage

class MediaStorage(S3Boto3Storage):
    location = "media"

class StaticFileStorage(S3StaticStorage):
    location = "static"
```

{% raw %}

- 장고 템플릿 js 파일 로케이션을 `{% static 'js/____.js' %}` 식으로 변경 
- 그리고 `STATIC_URL` 자리에 우리 S3 객체들이 담긴 URL을 넣어주었더니 js파일을 `127.0.0.1`이 아니라 `https://allbooks-choi-2.s3.amazonaws.com/static/js/getCookie.js` 이런 식으로 위치를 제대로 찾아가는 모습을 볼 수 있다.

{% endraw %}
