---
aliases: 
tags: 
description:
title: Domain Name Server (DNS)
created: 2024-09-02T10:48:43
updated: 2024-09-02T11:10:16
---
<https://medium.com/it-security-in-plain-english/dns-records-explained-a-beginners-guide-to-internet-routing-5cb15fa16593>

## DNS는 도메인 네임을 IP주소로 변환하는 과정

하지만 전 세계 도메인 네임을 하나의 서버가 전부 관리할 수는 없기 때문에 Top Level Domain (.com, .net 등) Server가 Authoritative Server에게 대신하여 요청을 전달한다. 이때 필요한 것이 ==DNS Records== 이다.

## DNS Records

- A Record: 도메인 이름을 IPv4 주소와 매핑한다.
- AAAA Record: 도메인 이름을 IPv6 주소와 매핑한다.
- CNAME Record: Canonical Name Record 라는 뜻으로, IP 주소 대신에 또 다른 도메인 이름으로 매핑한다. 이 레코드를 사용하면 Recursive Resolver가 IP 주소를 매핑한 DNS 서버를 찾을 때까지 순회하게 된다. 대체로 서브도메인에 CNAME Record를 활용하며, 예를 들어 `www.example.com`을 CNAME으로 등록하면 Recursive Resolver는 `example.com`으로 다시 DNS 쿼리를 하게 된다.
- NS Record: 

---

## Indices

- [[dns 서버에 대해서 설명해보세요]]
- [[무료 도메인 네임서버 DNS]]
- [[DNS 레코드 중 CNAME Record를 통해 URL이 호스트 될 때 발생하는 일]]
- [[DNS 호스팅 업체에 도메인 네임 등록 사례]]
