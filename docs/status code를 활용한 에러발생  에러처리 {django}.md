---
aliases: 
tags: 
description:
created: 2023-06-19T14:01:05
updated: 2023-07-15T21:33:03
title: status code를 활용한 에러발생  에러처리 {django}
---

# code snippet from `delete_product`

Code snippet from `products.views.delete_product`
<iframe src="https://github.com/ESTsoft-Book-Project/bookstore/blob/523366aa0e8eb61c16cbde90cce28190cbba8883/products/views.py#L62-L76" allow="fullscreen" allowfullscreen="" style="height:100%;width:100%; aspect-ratio: 16 / 9; "></iframe>

`delete_product` 하나만 봐도 HTML을 응답으로 보내는 줄은 하나밖에 안된다. 나머지는 전부 `JsonResponse`로 보내는 것을 볼 수 있다. 요청의 결과로 `result`와 `statusCode` 이렇게 나뉘어서 작성하고 있는데, 지금 프론트엔드 코드를 보면 `result`는 따로 사용을 하지 않기는 하다. `statusCode`만을 사용하여 요청결과를 숫자로 의미부여하는 모습을 볼 수 있다.

<iframe src="https://github.com/ESTsoft-Book-Project/bookstore/blob/0667d3eeded26793efe96c0f0243096822fb13dd/templates/book_detail.html#L39-L62" allow="fullscreen" allowfullscreen="" style="height:100%;width:100%; aspect-ratio: 16 / 9; "></iframe>

그렇다면 이제 `statusCode`가 각각 의미하는 바에 대하여 알아야겠다.

# status code

[moz.org](https://github.com/ESTsoft-Book-Project/bookstore/blob/0667d3eeded26793efe96c0f0243096822fb13dd/templates/book_detail.html#L39-L62)

HTTP response status codes indicate whether a specific [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) request has been successfully completed. Responses are grouped in five classes:

1. [Informational responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#information_responses) (`100` – `199`)
2. [Successful responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#successful_responses) (`200` – `299`)
3. [Redirection messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#redirection_messages) (`300` – `399`)
4. [Client error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses) (`400` – `499`)
5. [Server error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#server_error_responses) (`500` – `599`)

The status codes listed below are defined by [RFC 9110](https://httpwg.org/specs/rfc9110.html#overview.of.status.codes).

자주 쓰는 응답코드부터 보자.

TODO: 번역만 남기고 원문은 제거할 것

## Information responses

[`100 Continue`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/100)

This interim response indicates that the client should continue the request or ignore the response if the request is already finished.

[`101 Switching Protocols`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/101)

This code is sent in response to an [`Upgrade`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Upgrade) request header from the client and indicates the protocol the server is switching to.

[`102 Processing`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/102) ([WebDAV](https://developer.mozilla.org/en-US/docs/Glossary/WebDAV))

This code indicates that the server has received and is processing the request, but no response is available yet.

## Succesful responses

[`200 OK`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200)

The request succeeded. The result meaning of "success" depends on the HTTP method:  
해당 요청이 성공했습니다. "성공"의 의미는 HTTP 별로 다릅니다:

- `GET`: The resource has been fetched and transmitted in the message body.
	- `GET`: 요청하신 리소스가 성공적으로 메시지 바디에 담아 전송됐습니다.
- [`HEAD`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD): The representation headers are included in the response without any message body.
	- `HEAD`: 메시지 바디 없이 헤더에 응답이 들어있습니다. 
	- `HEAD` 자체는 `GET` 응답에 바디를 제외한 헤더만 요청하는 메서드이다. 예를 들면 대용량 파일을 요청할 경우 실제로 다운로드 하지 않고도 파일 크기를 [`Content-Length`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Length) 헤더를 통하여 알 수 있다.
- `PUT` or `POST`: The resource describing the result of the action is transmitted in the message body.
	- `PUT` or `POST`: 요청한 리소스와 바디를 잘 전달받았습니다.

[`201 Created`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/201)

The request succeeded, and a new resource was created as a result. This is typically the response sent after `POST` requests, or some `PUT` requests.  
요청이 성공했고, 그 결과로 새 리소스가 생성됐습니다. 주로 `POST` 혹은 `PUT` 메서드에 의한 성공코드로 활용됩니다.

[`202 Accepted`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/202)

The request has been received but not yet acted upon. It is noncommittal, since there is no way in HTTP to later send an asynchronous response indicating the outcome of the request. It is intended for cases where another process or server handles the request, or for batch processing.  
요청이 성공적으로 받아들여졌지만 아직 처리되지 않은 상태입니다. HTTP에서는 나중에 요청 결과를 나타내는 비동기 응답을 전송할 방법이 없으므로 *비*커밋 상태입니다. 다른 프로세스나 서버가 요청을 처리할 경우 혹은 배치 프로세스(일괄처리)에서 사용됩니다.

[`204 No Content`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/204)

There is no content to send for this request, but the headers may be useful. The user agent may update its cached headers for this resource with the new ones.  
이 요청에 대해 전송할 콘텐츠가 없지만 헤더가 유용할 수 있습니다. 사용자 에이전트는 이 리소스에 대해 캐시된 헤더를 새 헤더로 업데이트할 수 있습니다.

## [Client error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses)

[`400 Bad Request`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/400)

The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid request message framing, or deceptive request routing).

[`401 Unauthorized`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/401)

Although the HTTP standard specifies "unauthorized", semantically this response means "unauthenticated". That is, the client must authenticate itself to get the requested response.

[`402 Payment Required`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/402) Experimental

This response code is reserved for future use. The initial aim for creating this code was using it for digital payment systems, however this status code is used very rarely and no standard convention exists.

[`403 Forbidden`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/403)

The client does not have access rights to the content; that is, it is unauthorized, so the server is refusing to give the requested resource. Unlike `401 Unauthorized`, the client's identity is known to the server.

[`404 Not Found`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404)

The server cannot find the requested resource. In the browser, this means the URL is not recognized. In an API, this can also mean that the endpoint is valid but the resource itself does not exist. Servers may also send this response instead of `403 Forbidden` to hide the existence of a resource from an unauthorized client. This response code is probably the most well known due to its frequent occurrence on the web.

[`405 Method Not Allowed`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/405)

The request method is known by the server but is not supported by the target resource. For example, an API may not allow calling `DELETE` to remove a resource.

[`406 Not Acceptable`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/406)

This response is sent when the web server, after performing [server-driven content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation#server-driven_negotiation), doesn't find any content that conforms to the criteria given by the user agent.

[`407 Proxy Authentication Required`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/407)

This is similar to `401 Unauthorized` but authentication is needed to be done by a proxy.

[`408 Request Timeout`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408)

This response is sent on an idle connection by some servers, even without any previous request by the client. It means that the server would like to shut down this unused connection. This response is used much more since some browsers, like Chrome, Firefox 27+, or IE9, use HTTP pre-connection mechanisms to speed up surfing. Also note that some servers merely shut down the connection without sending this message.

[`409 Conflict`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/409)

This response is sent when a request conflicts with the current state of the server.

[`410 Gone`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/410)

This response is sent when the requested content has been permanently deleted from server, with no forwarding address. Clients are expected to remove their caches and links to the resource. The HTTP specification intends this status code to be used for "limited-time, promotional services". APIs should not feel compelled to indicate resources that have been deleted with this status code.

[`411 Length Required`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/411)

Server rejected the request because the `Content-Length` header field is not defined and the server requires it.

[`412 Precondition Failed`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/412)

The client has indicated preconditions in its headers which the server does not meet.

[`413 Payload Too Large`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/413)

Request entity is larger than limits defined by server. The server might close the connection or return an `Retry-After` header field.

[`414 URI Too Long`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/414)

The URI requested by the client is longer than the server is willing to interpret.

[`415 Unsupported Media Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/415)

The media format of the requested data is not supported by the server, so the server is rejecting the request.

[`416 Range Not Satisfiable`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/416)

The range specified by the `Range` header field in the request cannot be fulfilled. It's possible that the range is outside the size of the target URI's data.

[`417 Expectation Failed`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/417)

This response code means the expectation indicated by the `Expect` request header field cannot be met by the server.

[`418 I'm a teapot`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/418) ??????

The server refuses the attempt to brew coffee with a teapot.

[`421 Misdirected Request`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/421)

The request was directed at a server that is not able to produce a response. This can be sent by a server that is not configured to produce responses for the combination of scheme and authority that are included in the request URI.

## [Server error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#server_error_responses)

[`500 Internal Server Error`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500)

The server has encountered a situation it does not know how to handle.  
서버가 다룰 수 없는 문제에 맞닥뜨렸습니다.

[`501 Not Implemented`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/501)

The request method is not supported by the server and cannot be handled. The only methods that servers are required to support (and therefore that must not return this code) are `GET` and `HEAD`.

[`502 Bad Gateway`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/502)

This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response.

[`503 Service Unavailable`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/503)

The server is not ready to handle the request. Common causes are a server that is down for maintenance or that is overloaded. Note that together with this response, a user-friendly page explaining the problem should be sent. This response should be used for temporary conditions and the `Retry-After` HTTP header should, if possible, contain the estimated time before the recovery of the service. The webmaster must also take care about the caching-related headers that are sent along with this response, as these temporary condition responses should usually not be cached.  
서버가 요청을 처리할 준비가 되지 않았습니다. 일반적인 원인은 유지보수를 위해 서버가 다운되었거나 과부하가 걸린 경우입니다. 이 응답과 함께 문제를 설명하는 사용자 친화적인 페이지가 전송되어야 한다는 점에 유의하세요. 이 응답은 일시적인 상황에 사용해야 하며, 가능하면 `Retry-After` HTTP 헤더에 서비스 복구 전 예상 시간을 포함해야 합니다. 이러한 임시 조건 응답은 일반적으로 캐싱되지 않아야 하므로 웹 마스터는 이 응답과 함께 전송되는 캐싱 관련 헤더에도 주의를 기울여야 합니다.

[`504 Gateway Timeout`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/504)

This error response is given when the server is acting as a gateway and cannot get a response in time.

[`505 HTTP Version Not Supported`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/505)

The HTTP version used in the request is not supported by the server.

[`506 Variant Also Negotiates`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/506)

The server has an internal configuration error: the chosen variant resource is configured to engage in transparent content negotiation itself, and is therefore not a proper end point in the negotiation process.

# Google Cloud :: JSON Status Code

[cloud.google.com](https://cloud.google.com/storage/docs/json_api/v1/status-codes)

```json
404 Not Found

{
"error": {
 "errors": [
  {
   "domain": "global",
   "reason": "notFound",
   "message": "Not Found"
  }
 ],
 "code": 404,
 "message": "Not Found"
 }
}
```
