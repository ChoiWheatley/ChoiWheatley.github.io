---
aliases: 
tags: 
description:
title: Cookie
created: 2024-08-01T19:47:03
updated: 2024-08-01T19:47:06
---

### Sample `Set-Cookie` Header

```http
Set-Cookie: sessionId=abc123; Path=/; Secure; HttpOnly; SameSite=Lax; Max-Age=3600; Domain=api.example.com
```

### Breakdown of Attributes

- **`sessionId=abc123`:**  
  This is the name and value of the cookie. In this case, the cookie is named `sessionId` with a value of `abc123`.

- **`Path=/`:**  
  The cookie is valid for the entire domain and all its paths. It will be sent with requests to `https://api.example.com/`, `https://api.example.com/posts`, `https://api.example.com/posts/3`, etc.

- **`Secure`:**  
  The cookie will only be sent over HTTPS connections, ensuring that it is encrypted during transit.

- **`HttpOnly`:**  
  This attribute prevents JavaScript from accessing the cookie, enhancing security against XSS (Cross-Site Scripting) attacks.

- **`SameSite=Lax`:**  
  This attribute controls whether the cookie is sent with cross-site requests. `Lax` allows the cookie to be sent with top-level navigations, reducing CSRF (Cross-Site Request Forgery) risks. Alternatives are `Strict` (only same-site requests) and `None` (cross-site requests, must be used with `Secure`).

- **`Max-Age=3600`:**  
  The cookie will expire in 3600 seconds (1 hour) from the time it is set. Alternatively, you can use the `Expires` attribute to specify an absolute expiry date.

- **`Domain=api.example.com`:**  
  The cookie is valid for this specific domain. If you omit this attribute, the cookie will default to the domain that set it.

### Example with `Expires` Attribute

If you prefer to use the `Expires` attribute instead of `Max-Age`, here’s how it looks:

```http
Set-Cookie: sessionId=abc123; Path=/; Secure; HttpOnly; SameSite=Lax; Expires=Wed, 02 Aug 2024 10:23:30 GMT; Domain=api.example.com
```

### Usage Considerations

- **Path and Domain Scope:** Ensure the `Path` and `Domain` attributes are set correctly to match the scope where you want the cookie to be valid.
- **Security:** Use `Secure` and `HttpOnly` attributes whenever possible to enhance security.
- **SameSite Policy:** Choose an appropriate `SameSite` policy based on the application’s needs for cross-site cookie usage.

By configuring these attributes appropriately, you can ensure that your cookies are used safely and effectively within your application.
