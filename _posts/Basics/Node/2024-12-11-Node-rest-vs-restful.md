---
title: "REST와 RESTful의 차이와 원칙: API 설계의 기본"
excerpt: "REST의 핵심 원칙과 RESTful API 설계 방식을 이해하고, 실무에서 어떻게 활용할 수 있는지 자세히 알아봅니다."
categories:
  - Node
tags:
  - [REST, RESTful, Backend, API 설계]
permalink: /Node/rest-vs-restful/
toc: true
toc_sticky: true
date: 2024-12-11
last_modified_at: 2024-12-11
---

## REST와 RESTful의 기본 이해

REST(Representational State Transfer)와 RESTful API는 현대 웹 개발에서 필수적인 개념입니다. REST는 API 설계를 위한 아키텍처 스타일이고, RESTful은 이 원칙을 준수하는 방식으로 구현된 API를 의미합니다. 이번 글에서는 REST의 핵심 원칙과 RESTful API 설계 방법을 자세히 알아봅니다.

---

### REST의 핵심 원칙

REST는 특정 제약 조건을 기반으로 시스템을 설계합니다. RESTful한 시스템은 확장성과 유지보수성이 뛰어납니다. 주요 원칙은 다음과 같습니다:

#### 1. **클라이언트-서버 구조**
- 클라이언트와 서버는 명확히 분리되어야 합니다.
- 클라이언트는 UI를, 서버는 데이터와 비즈니스 로직을 담당합니다.

#### 2. **무상태성 (Stateless)**
- 서버는 클라이언트의 상태를 저장하지 않습니다.
- 요청마다 필요한 모든 정보를 포함해야 합니다.
  - 예: 요청 헤더에 인증 토큰 추가.

#### 3. **캐시 가능성 (Cacheable)**
- 응답 데이터는 명시적으로 캐시될 수 있어야 합니다.
- HTTP 헤더(`Cache-Control`, `ETag`)를 활용해 캐싱 정책을 정의합니다.

#### 4. **계층화된 시스템 (Layered System)**
- 클라이언트는 중간 서버(예: 로드 밸런서, 프록시)를 거쳐 요청을 처리할 수 있습니다.

#### 5. **통합된 인터페이스 (Uniform Interface)**
- 모든 자원은 일관된 방식으로 접근해야 하며, URI를 통해 고유하게 식별됩니다.

#### 6. **주문형 코드 (On-demand Code)**
- 필요 시 클라이언트에 코드를 동적으로 제공할 수 있습니다.

---

### RESTful API 설계의 주요 구성 요소

RESTful API는 REST 원칙을 실제로 구현한 것입니다. 이를 설계할 때는 다음 요소들을 고려해야 합니다.

#### 1. **자원의 설계**
- URI는 자원의 이름에 초점을 맞춰야 합니다.
  - 예: `/users`는 자원을 표현하지만, `/getUsers`는 행위 기반으로 잘못된 설계입니다.

#### 2. **HTTP 메서드의 활용**
- HTTP 메서드는 자원의 작업을 표현합니다.

| HTTP 메서드 | 의미                 | 예시                      |
|-------------|----------------------|---------------------------|
| GET         | 자원 조회           | `GET /users/123`          |
| POST        | 자원 생성           | `POST /users`             |
| PUT         | 자원 전체 수정       | `PUT /users/123`          |
| PATCH       | 자원 일부 수정       | `PATCH /users/123`        |
| DELETE      | 자원 삭제           | `DELETE /users/123`       |

#### 3. **URI 설계 규칙**
- **계층 구조 반영**:
  - 예: `/users/123/orders/456` (사용자 123의 주문 456)
- **필터와 정렬**:
  - 쿼리 파라미터를 활용.
    - 예: `GET /users?age=30&sort=name`

#### 4. **응답 데이터 형식**
- JSON을 주로 사용합니다.

  ```json
  {
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com"
  }
  ```

#### 5. **HTTP 상태 코드**
- 요청에 대한 결과를 상태 코드로 전달합니다.

| 상태 코드 | 의미                  |
|-----------|-----------------------|
| 200 OK    | 요청 성공             |
| 201 Created | 자원 생성 성공      |
| 400 Bad Request | 잘못된 요청     |
| 401 Unauthorized | 인증 실패      |
| 404 Not Found | 자원 없음         |

---

### REST와 RESTful의 차이

| **항목**       | **REST**                                | **RESTful**                          |
|----------------|-----------------------------------------|---------------------------------------|
| **정의**       | 아키텍처 스타일 (원칙과 제약 조건).     | REST 원칙을 준수한 서비스/API 구현.  |
| **의미**       | 이론적인 개념.                          | 실제 구현된 API.                     |

---

### RESTful API의 예시

#### 사용자 관리 API 설계

| 동작          | HTTP 메서드 | URI            | 설명                            |
|---------------|-------------|----------------|---------------------------------|
| 사용자 목록 조회 | GET         | `/users`       | 모든 사용자 조회.               |
| 특정 사용자 조회 | GET         | `/users/{id}`  | 특정 사용자 정보 조회.           |
| 사용자 생성    | POST        | `/users`       | 새로운 사용자 생성.             |
| 사용자 수정    | PUT         | `/users/{id}`  | 특정 사용자 정보 전체 수정.      |
| 사용자 삭제    | DELETE      | `/users/{id}`  | 특정 사용자 삭제.               |

#### 응답 예시
1. **GET /users/123**

   ```json
   {
     "id": 123,
     "name": "John Doe",
     "email": "john@example.com"
   }
   ```

2. **POST /users**
   요청:

   ```json
   {
     "name": "Jane Doe",
     "email": "jane@example.com"
   }
   ```
   
   응답:

   ```json
   {
     "id": 124,
     "name": "Jane Doe",
     "email": "jane@example.com"
   }
   ```

---

### RESTful API의 장단점

#### **장점**
1. 범용적인 HTTP 프로토콜 사용.
2. 클라이언트-서버 구조로 확장성과 유지보수성 향상.
3. 명확한 HTTP 메서드와 상태 코드 활용.

#### **단점**
1. 복잡한 관계를 표현하기 어려움.
2. 실시간 통신에는 부적합 (WebSocket, MQTT 추천).
3. HATEOAS 구현 부족.

---

REST와 RESTful API 설계는 현대적인 백엔드 시스템 개발의 핵심입니다. 기본 원칙과 설계 방식을 이해하면 확장성 있고 유지보수하기 쉬운 시스템을 구축할 수 있습니다.
