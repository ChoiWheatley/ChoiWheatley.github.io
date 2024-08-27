---
aliases: 
tags: 
description:
created: 2023-07-02T21:53:09
updated: 2024-08-27T13:37:22
title: "HTTP header CORS policy - 'Access-Control-Allow-Origin'"
---
- [sof](https://stackoverflow.com/questions/28046422/django-cors-headers-not-work)
- [[2023-11-18 CORS 정면추돌]]
- [[CORS]]
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

## GPT 설전

> 프론트엔드와 백엔드가 따로 구현하는 과정에서  Cross Origin Resource Sharing 에러가 발생해. 프론트엔드와 백엔드 호스트가 달라서 생긴 문제인 것 같은데 백엔드에서 "Access-Control-Allow-Origin" 헤더를 `"*"` 로 보내고 있는데도 문제가 여전해. 혹시 프론트엔드에서 Referrer Policy가 same-origin으로 설정되어 있는 문제 아닐까?

CORS(Cross-Origin Resource Sharing) 에러는 주로 클라이언트(프론트엔드)와 서버(백엔드) 간의 출처(Origin)가 다른 경우에 발생합니다. 현재 상황에서 백엔드가 `Access-Control-Allow-Origin` 헤더에 `*`를 사용하고 있음에도 문제가 계속 발생하는 것으로 보아, 여러 가지 원인이 있을 수 있습니다. Referrer Policy가 `same-origin`으로 설정되어 있다면 CORS 문제를 유발할 수 있는 상황도 고려해 볼 필요가 있습니다.

다음은 이 문제를 해결하기 위한 주요 확인 사항들입니다:

### 1. **Referrer Policy와 CORS의 관계**

   Referrer Policy가 `same-origin`으로 설정되어 있다면, 브라우저는 다른 출처(Origin)에 요청을 보낼 때 원래의 Referrer 헤더를 제거할 수 있습니다. 하지만 이것이 CORS 에러의 직접적인 원인은 아닙니다. Referrer Policy는 기본적으로 어떤 출처 정보가 서버에 보내질지를 제어하는 설정으로, CORS와 직접적인 상관은 없습니다. 따라서 Referrer Policy 설정 자체는 중요한 요소가 아닐 가능성이 큽니다.

### 2. **Credentials (인증 정보) 전송 여부**

   만약 프론트엔드에서 쿠키와 같은 인증 정보를 서버로 보내고 있다면, 서버에서 `Access-Control-Allow-Origin`을 `*`로 설정하는 것은 동작하지 않습니다. 이 경우 백엔드에서 반드시 특정 도메인으로 `Access-Control-Allow-Origin`을 설정하고, `Access-Control-Allow-Credentials: true` 헤더도 함께 설정해야 합니다.

   프론트엔드 요청에서 다음과 같이 설정이 되어 있는지 확인해 보세요:

   ```js
   fetch('API_ENDPOINT', {
     credentials: 'include',  // 쿠키 전송 시 'include'로 설정
     ...
   })
   ```

   백엔드에서 다음과 같이 설정해야 합니다:

   ```http
   Access-Control-Allow-Origin: https://your-frontend-domain.com
   Access-Control-Allow-Credentials: true
   ```

   여기서 `*` 대신 특정 도메인을 명시해야 인증 정보가 포함된 요청이 성공적으로 전송됩니다.

### 3. **프리플라이트 요청 (Preflight Request) 확인**

   일부 CORS 요청에서는 브라우저가 실제 요청을 보내기 전에 OPTIONS 메서드를 사용하여 프리플라이트 요청을 보냅니다. 서버가 이 프리플라이트 요청에 적절히 응답하지 못할 경우에도 CORS 에러가 발생할 수 있습니다.

   서버에서 OPTIONS 요청에 대해 적절한 CORS 헤더를 반환하는지 확인하세요:

   ```http
   Access-Control-Allow-Methods: GET, POST, OPTIONS
   Access-Control-Allow-Headers: Content-Type
   ```

### 4. **브라우저 캐시 확인**

   CORS 에러가 간헐적으로 발생한다면, 브라우저 캐시나 프록시 캐시가 원인일 수도 있습니다. 캐시를 비우고 다시 시도해 보세요.

### 5. **네트워크와 브라우저 콘솔 로그 확인**

   브라우저의 개발자 도구에서 네트워크 탭을 열어 실제로 어떤 요청이 어떻게 전송되고 있는지 확인해 보세요. 또한, 콘솔 로그에 나오는 CORS 관련 에러 메시지를 확인하면 좀 더 구체적인 원인을 파악하는 데 도움이 될 수 있습니다.

위의 점들을 모두 확인한 후에도 문제가 해결되지 않으면, 네트워크 흐름을 좀 더 자세히 추적하거나, 사용하고 있는 웹 서버와 프레임워크의 CORS 설정을 재검토할 필요가 있을 수 있습니다.
