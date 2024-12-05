---
title: "HTTP 헤더란 무엇인가? 웹 개발자를 위한 필수 가이드"
excerpt: "HTTP 헤더의 역할, 종류, 주요 헤더의 사용법, 그리고 API 설계 시 활용 사례를 알아봅니다."
categories:
  - Web
tags:
  - [HTTP, Web, REST API, Headers]
permalink: /Web/http-headers-guide/
toc: true
toc_sticky: true
date: 2024-12-5
last_modified_at: 2024-12-5
---

## HTTP 헤더란?

HTTP 헤더는 클라이언트(예: 브라우저)와 서버 간의 요청(Request) 및 응답(Response) 시 데이터를 주고받는 데 사용되는 메타정보입니다. 헤더를 통해 데이터의 형식, 인증, 캐싱 정책 등 다양한 정보를 설정하거나 전달할 수 있습니다.

---

## HTTP 헤더의 주요 분류

### 1. 일반 헤더 (General Header)
요청과 응답 모두에서 사용 가능한 공통 헤더입니다.  
**예:** `Cache-Control`, `Connection`

### 2. 요청 헤더 (Request Header)
클라이언트가 서버에 추가 정보를 제공할 때 사용됩니다.  
**예:** `Accept`, `Authorization`, `User-Agent`

### 3. 응답 헤더 (Response Header)
서버가 클라이언트에게 응답을 전달할 때 추가 정보를 제공할 때 사용됩니다.  
**예:** `Content-Type`, `Set-Cookie`, `Server`

### 4. 엔터티 헤더 (Entity Header)
요청 또는 응답 본문 데이터의 특성을 설명하는 데 사용됩니다.  
**예:** `Content-Length`, `Content-Encoding`

---

## 자주 사용하는 요청 헤더

### `Accept`
클라이언트가 서버로부터 어떤 형식의 데이터를 받을 수 있는지 지정합니다.
```http
Accept: text/html,application/json
```

### `Accept-Language`
클라이언트가 선호하는 언어를 지정합니다.
```http
Accept-Language: en-US,ko-KR;q=0.8
```
- `q`는 선호도(priority)를 나타냅니다.

### `Authorization`
서버 인증 정보를 전달합니다.
```http
Authorization: Bearer <token>
```

### `Content-Type`
요청 본문의 데이터 형식을 지정합니다.
```http
Content-Type: application/json
```

### `User-Agent`
클라이언트의 소프트웨어 환경 정보를 제공합니다.
```http
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
```

---

## 자주 사용하는 응답 헤더

### `Content-Type`
응답 데이터의 MIME 타입을 지정합니다.
```http
Content-Type: application/json
```

### `Content-Length`
응답 본문의 크기를 바이트 단위로 나타냅니다.
```http
Content-Length: 348
```

### `Set-Cookie`
클라이언트에 쿠키를 설정합니다.
```http
Set-Cookie: sessionId=abc123; Path=/; Secure; HttpOnly
```

### `Cache-Control`
캐싱 동작을 제어합니다.
```http
Cache-Control: no-cache, no-store, must-revalidate
```

### `Location`
리다이렉션 대상 URL을 제공합니다.
```http
Location: https://example.com/login
```

---

## HTTP 헤더 활용 사례

### 1. API 데이터 형식 조건 처리
API 문서에서 클라이언트가 특정 형식의 데이터를 요청하는 경우:
- JSON 데이터를 요청할 때:
```http
Accept: application/json
```

- XML 데이터를 요청할 때:
```http
Accept: application/xml
```

### 2. API 버전 관리
API 버전 관리를 위해 헤더를 활용할 수 있습니다.
```http
Accept: application/vnd.example.v1+json
```

### 3. 조건부 요청 (Conditional Requests)
- 변경된 데이터만 요청:
```http
If-Modified-Since: Mon, 29 Nov 2024 10:00:00 GMT
```

- 캐시 유효성 확인:
```http
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d8"
```

---

## 결론

HTTP 헤더는 클라이언트와 서버 간의 통신을 세부적으로 제어하는 데 핵심적인 역할을 합니다. WebAPI 설계 및 사용 시 헤더를 이해하고 적절히 활용하면 효율적이고 확장 가능한 애플리케이션을 구축할 수 있습니다.

더 알고 싶은 점이 있거나 궁금한 점이 있다면 댓글로 남겨주세요! 😊
