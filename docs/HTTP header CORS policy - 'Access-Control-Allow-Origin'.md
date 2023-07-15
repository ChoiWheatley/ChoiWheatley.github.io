---
aliases: 
tags: 
description:
created: 2023-07-02T21:53:09
updated: 2023-07-15T21:33:04
title: "HTTP header CORS policy - 'Access-Control-Allow-Origin'"
---
- [sof](https://stackoverflow.com/questions/28046422/django-cors-headers-not-work)
- S3 CORS Policy를 일단 다 때려넣어봤다

```json
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "HEAD",
            "PUT",
            "GET",
            "POST",
            "DELETE"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": [
            "x-amz-request-id",
            "x-amz-server-side-encryption",
            "x-amz-version-id",
            "Content-Length",
            "Content-Type",
            "Connection",
            "Date",
            "ETag",
            "x-amz-delete-marker"
        ]
    }
]
```

그리고 미들웨어를 하나 만들었다. ~~django-cors-headers는 쓰레기다~~ 다음 미들웨어는 모든 response 헤더에 `Acess-Control-Allow-Headers`를 추가한다.

```python
from django import http

class CorsMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response
    
    def __call__(self, request):
        response = self.get_response(request)
        if (request.method == "OPTIONS"  and "HTTP_ACCESS_CONTROL_REQUEST_METHOD" in request.META):
            response = http.HttpResponse()
            response["Content-Length"] = "0"
            response["Access-Control-Max-Age"] = 86400
        response["Access-Control-Allow-Origin"] = "*"
        response["Access-Control-Allow-Methods"] = "DELETE, GET, OPTIONS, PATCH, POST, PUT"
        response["Access-Control-Allow-Headers"] = "accept, accept-encoding, authorization, content-type, dnt, origin, user-agent, x-csrftoken, x-requested-with"
        return response

```
