---
title: "Express-Mongo-Sanitize: NoSQL Injection 방지하기"
excerpt: "Express 애플리케이션에서 MongoDB를 안전하게 사용하기 위한 express-mongo-sanitize의 설치와 사용법을 소개합니다."
categories:
  - Node
  - Security
tags:
  - [Node.js, Express, MongoDB, 보안, NoSQL Injection]
permalink: /node/express-mongo-sanitize/
toc: true
toc_sticky: true
date: 2025-01-10
last_modified_at: 2025-01-10
---

## express-mongo-sanitize란?

`express-mongo-sanitize`는 **Express** 애플리케이션에서 MongoDB를 사용하는 경우, **NoSQL 인젝션 공격**을 방지하기 위한 미들웨어입니다. 이 라이브러리는 클라이언트에서 전송된 요청(req.body, req.query, req.params)에서 MongoDB의 `$` 연산자나 `.` 연산자를 제거하여 악의적인 데이터가 데이터베이스에 영향을 미치지 않도록 합니다.

---

### 1. NoSQL Injection이란?

MongoDB는 `$`와 `.` 문법을 통해 다양한 쿼리 연산을 지원합니다. 하지만 공격자가 이 문법을 악용해 의도치 않은 데이터를 삽입하거나 쿼리를 변조할 수 있습니다.

#### 예시 공격:

```javascript
// 클라이언트에서 의도적으로 악의적인 데이터를 전송:
{
  "username": { "$gt": "" },
  "password": "12345"
}

// MongoDB에 전달되는 쿼리:
db.users.find({ username: { $gt: "" }, password: "12345" })
```

위의 쿼리는 모든 사용자를 반환하도록 조작된 상태입니다.

---

### 2. express-mongo-sanitize의 동작 원리

`express-mongo-sanitize`는 요청 본문(req.body), 쿼리(req.query), 또는 URL 파라미터(req.params) 내에서 `$` 또는 `.`이 포함된 키를 탐지하고 이를 제거하거나, 필요시 애플리케이션에서 이를 차단합니다.

#### 주요 특징:
- `$`나 `.`으로 시작하거나 포함된 필드를 제거.
- 애플리케이션의 데이터 무결성과 보안을 강화.
- 설치 및 사용이 간단.

---

### 3. 설치 방법

```bash
npm install express-mongo-sanitize
```

---

### 4. 사용 방법

#### 기본 사용:

```javascript
const express = require('express');
const mongoSanitize = require('express-mongo-sanitize');

const app = express();

// JSON 및 URL-encoded 데이터 파싱
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// express-mongo-sanitize 미들웨어 추가
app.use(mongoSanitize());

// 라우트 예제
app.post('/data', (req, res) => {
  console.log(req.body); // 요청 데이터가 sanitize 처리됨
  res.send('Data processed safely');
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

#### 커스터마이징:

```javascript
app.use(mongoSanitize({
  replaceWith: '_', // $나 .을 _로 대체
}));
```

#### 제거 대신 차단:

```javascript
app.use(mongoSanitize({
  onSanitize: ({ req, key }) => {
    console.warn(`This request contains a suspicious key: ${key}`);
  },
}));
```

---

### 5. 옵션

- **`replaceWith`**: 기본적으로 `$`나 `.`이 제거되지만, 대체 문자를 지정할 수 있습니다.
  ```javascript
  { replaceWith: '_' }
  ```
- **`onSanitize`**: 필터링된 키가 있을 때 호출되는 콜백 함수.
  ```javascript
  onSanitize: ({ req, key }) => { ... }
  ```

---

### 6. 주의사항

- 이 미들웨어는 클라이언트 요청에 포함된 MongoDB 관련 키만 제거하며, 데이터 검증(validation)이나 권한 확인(auth) 등의 역할은 하지 않습니다.
- 데이터베이스 계층에서 ORM/ODM (예: Mongoose)의 데이터 검증과 함께 사용하는 것이 좋습니다.

---

### 7. 관련 보안 도구

- **Helmet.js**: HTTP 헤더를 통해 Express 앱 보안을 강화.
- **express-validator**: 입력 데이터 검증.
- **rate-limiter-flexible**: 요청 속도 제한을 통한 DoS/DDoS 방지.

`express-mongo-sanitize`는 NoSQL 인젝션 방지의 첫 단계로, 다른 보안 도구와 함께 사용하는 것이 권장됩니다. 😊
