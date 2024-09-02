---
aliases: 
tags: 
description:
title: DNS 레코드 중 CNAME Record를 통해 URL이 호스트 될 때 발생하는 일
created: 2024-09-02T11:23:36
updated: 2024-09-02T11:23:57
---

> DNS 레코드에 대해서 조사하고 있는데, CNAME Record에 호스트되는 URL을 입력하게 되었을 때 발생하는 일에 대해서 알려줘

CNAME (Canonical Name) 레코드는 DNS(Domain Name System)에서 도메인 이름을 다른 도메인 이름으로 매핑하는 데 사용됩니다. 쉽게 말해, CNAME 레코드를 설정하면 하나의 도메인 이름이 다른 도메인 이름을 가리키도록 할 수 있습니다. 예를 들어, `www.example.com`을 `example.com`에 매핑하거나 `blog.example.com`을 외부 서비스의 도메인(`hostedblog.com`)에 매핑할 수 있습니다.

CNAME 레코드를 통해 URL이 호스트될 때 발생하는 일은 다음과 같습니다:

### 1. CNAME 레코드 조회

사용자가 브라우저에 CNAME 레코드가 설정된 도메인 이름(예: `www.example.com`)을 입력하면, DNS 서버는 해당 도메인 이름에 대해 CNAME 레코드를 조회합니다. 

### 2. 실제 호스트로 변환

DNS 서버는 CNAME 레코드에 따라 도메인 이름을 다른 도메인 이름으로 변환합니다. 예를 들어, `www.example.com`이 `example.com`에 대한 CNAME 레코드로 설정되어 있으면, DNS 서버는 `www.example.com`을 `example.com`으로 변환합니다.

### 3. A 레코드 조회

변환된 도메인 이름(예: `example.com`)에 대해 다시 DNS 쿼리가 수행됩니다. 이때 `example.com`의 A 레코드(또는 AAAA 레코드)가 조회되어 실제 IP 주소가 반환됩니다. IP 주소는 클라이언트에게 반환되어, 클라이언트는 해당 IP 주소로 요청을 보냅니다.

### 4. 클라이언트의 연결

클라이언트는 최종적으로 반환된 IP 주소로 연결을 시도합니다. 여기서 중요한 점은 클라이언트는 CNAME 레코드를 통해 변환된 최종 도메인 이름이 아닌, 최초로 입력된 도메인 이름(예: `www.example.com`)을 여전히 사용한다는 점입니다.

### CNAME 레코드 사용 시 고려 사항

- **CNAME 레코드에는 A 레코드를 함께 사용할 수 없습니다**. 동일한 도메인에 대해 A 레코드와 CNAME 레코드를 동시에 설정하는 것은 불가능합니다.
- **CNAME 레코드는 서브도메인에 주로 사용**됩니다. 루트 도메인(예: `example.com`)에 CNAME 레코드를 설정하는 것은 권장되지 않습니다.
- **성능**: CNAME 레코드는 추가적인 DNS 조회를 발생시키므로, 응답 시간에 약간의 지연이 생길 수 있습니다.
- **CNAME 체인**: 여러 CNAME 레코드가 연속적으로 체인으로 설정되면, 결국 최종적으로 IP 주소로 변환되기까지의 시간이 길어질 수 있습니다.

따라서 CNAME 레코드를 사용하면 도메인 간의 매핑을 유연하게 관리할 수 있지만, 불필요한 체인을 만들지 않도록 주의해야 합니다.

> CNAME 레코드를 사용하여 조회를 시도할때 Recursive Resolver가 이 작업을 수행하는거야?

네, 맞습니다. CNAME 레코드를 사용한 조회 작업은 **Recursive Resolver**가 수행합니다. 이 과정을 조금 더 상세히 설명하자면:

### Recursive Resolver의 역할

Recursive Resolver는 사용자가 도메인 이름을 입력하면, 해당 도메인의 IP 주소를 찾기 위해 여러 DNS 서버를 순차적으로 조회하는 역할을 합니다. 이 과정에서 Recursive Resolver는 CNAME 레코드를 처리하는 것도 포함됩니다.

### CNAME 조회 시 Recursive Resolver의 동작 과정

1. **도메인 이름 조회 요청**: 사용자가 `www.example.com`과 같은 도메인 이름을 입력하면, 이 요청은 Recursive Resolver에게 전달됩니다.

2. **DNS 서버로부터의 정보 조회**: Recursive Resolver는 해당 도메인에 대한 정보(예: A 레코드 또는 CNAME 레코드)를 찾기 위해 DNS 서버(일반적으로 권한 있는 네임서버, 즉 Authoritative Name Server)에 쿼리를 보냅니다.

3. **CNAME 레코드 확인**: Authoritative Name Server는 `www.example.com`에 대한 CNAME 레코드를 가지고 있다고 가정해봅시다. 이 경우, 해당 서버는 `www.example.com`이 `example.com`을 가리킨다는 정보를 Recursive Resolver에게 반환합니다.

4. **새로운 쿼리 수행**: Recursive Resolver는 이제 `example.com`에 대해 다시 쿼리를 수행합니다. 이 때, `example.com`에 대한 A 레코드(또는 AAAA 레코드)를 조회하게 되고, 최종적으로 이 도메인 이름에 연결된 IP 주소를 획득하게 됩니다.

5. **최종 IP 주소 반환**: Recursive Resolver는 이 IP 주소를 클라이언트(즉, 웹 브라우저나 애플리케이션)에게 반환합니다. 클라이언트는 이 IP 주소를 사용해 실제로 해당 서버에 연결을 시도하게 됩니다.

### 요약

- **Recursive Resolver**는 사용자가 입력한 도메인 이름에 대해 최종 IP 주소를 찾기 위해 여러 단계를 수행하며, 이 과정에서 CNAME 레코드를 처리합니다.
- Recursive Resolver는 CNAME 레코드를 확인한 후, 해당 CNAME이 가리키는 새로운 도메인 이름에 대해 추가적인 DNS 조회를 수행합니다.
- 최종적으로, Recursive Resolver는 최종 도메인 이름에 대한 A 레코드(또는 AAAA 레코드)를 조회해 클라이언트에게 반환합니다.

따라서, CNAME 레코드와 관련된 모든 처리 과정에서 Recursive Resolver가 중요한 역할을 합니다.

