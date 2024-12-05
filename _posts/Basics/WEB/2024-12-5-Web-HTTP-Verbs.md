---
title: "HTTP Verbs의 종류와 사용 목적"
excerpt: "HTTP Verbs(GET, POST, PUT 등)의 역할과 사용 사례를 이해하고, 안전성, 멱등성, 캐시 가능성에 대한 개념도 살펴봅니다."
categories:
  - Web
tags:
  - [HTTP, API, REST, GET, POST, Web]
permalink: /Web/HTTP-Verbs/
toc: true
toc_sticky: true
date: 2024-12-5
last_modified_at: 2024-12-5
---

## HTTP Verbs란?

HTTP Verbs(또는 Methods)는 클라이언트가 서버에 요청할 때 수행하고자 하는 작업의 종류를 명시합니다. 아래는 각 메서드의 역할, 사용 사례, 주요 속성을 설명한 내용입니다.

---

### **1. GET**
- **목적:** 리소스 조회
- **설명:** 
  - 서버에서 특정 리소스(데이터)를 가져옵니다.
  - 데이터를 변경하지 않습니다. (안전하고, 멱등성 있음)
- **예시:**
  ```http
  GET /articles HTTP/1.1
  ```

---

### **2. POST**
- **목적:** 리소스 생성
- **설명:**
  - 서버에 데이터를 전송하여 새로운 리소스를 생성하거나 특정 동작을 수행합니다.
  - 동일 요청을 여러 번 보내면 리소스가 중복 생성될 수 있습니다. (멱등성 없음)
- **예시:**
  ```http
  POST /users HTTP/1.1
  Content-Type: application/json

  {
    "name": "John",
    "email": "john@example.com"
  }
  ```

---

### **3. PUT**
- **목적:** 리소스 업데이트(또는 생성)
- **설명:**
  - 지정된 리소스를 업데이트하거나, 리소스가 없으면 새로 생성합니다.
  - 멱등성을 보장합니다.
- **예시:**
  ```http
  PUT /users/1 HTTP/1.1
  Content-Type: application/json

  {
    "name": "Jane",
    "email": "jane@example.com"
  }
  ```

---

### **4. PATCH**
- **목적:** 리소스 부분 업데이트
- **설명:**
  - 리소스의 일부 데이터를 수정합니다.
  - 변경이 필요한 데이터만 요청 본문에 포함.
- **예시:**
  ```http
  PATCH /users/1 HTTP/1.1
  Content-Type: application/json

  {
    "name": "Jane"
  }
  ```

---

### **5. DELETE**
- **목적:** 리소스 삭제
- **설명:**
  - 지정된 리소스를 서버에서 삭제합니다.
  - 멱등성을 보장합니다.
- **예시:**
  ```http
  DELETE /users/1 HTTP/1.1
  ```

---

### **6. HEAD**
- **목적:** 리소스의 헤더 정보 조회
- **설명:**
  - GET과 유사하지만, 응답 본문(body)은 포함하지 않고 헤더만 반환.
- **예시:**
  ```http
  HEAD /articles HTTP/1.1
  ```

---

### **7. OPTIONS**
- **목적:** 서버에서 지원하는 메서드 확인
- **설명:**
  - 특정 URL에서 허용되는 HTTP 메서드를 확인합니다.
- **예시:**
  ```http
  OPTIONS /users HTTP/1.1
  ```

---

### **8. TRACE**
- **목적:** 요청 경로 추적
- **설명:**
  - 클라이언트와 서버 간의 요청 전달 경로를 테스트하는 데 사용.

---

### **9. CONNECT**
- **목적:** TCP 터널링
- **설명:**
  - 클라이언트와 서버 간의 터널을 생성하여 HTTPS와 같은 보안 연결을 설정합니다.

---

## HTTP 메서드의 주요 속성

| **메서드** | **안전성** | **멱등성** | **캐시 가능성** |
|------------|------------|------------|-----------------|
| GET        | ✅         | ✅         | ✅              |
| POST       | ❌         | ❌         | ❌              |
| PUT        | ❌         | ✅         | ❌              |
| PATCH      | ❌         | ✅ (조건적)| ❌              |
| DELETE     | ❌         | ✅         | ❌              |
| HEAD       | ✅         | ✅         | ✅              |
| OPTIONS    | ✅         | ✅         | ❌              |
| TRACE      | ✅         | ✅         | ❌              |
| CONNECT    | ❌         | ❌         | ❌              |

HTTP Verbs를 올바르게 이해하면 RESTful API 설계와 클라이언트-서버 간의 효율적인 통신 구현에 큰 도움이 됩니다.
