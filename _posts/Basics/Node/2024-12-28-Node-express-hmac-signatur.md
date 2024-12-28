---
title: "Express에서 HMAC 서명 이해하기: Node.js를 활용한 데이터 무결성 검증"
excerpt: "HMAC 서명을 활용한 데이터 인증과 Express 기반의 API 보안을 위한 구현 방법을 자세히 알아봅니다."
categories:
  - Node
  - Express
  - Security
tags:
  - [HMAC, Node.js, Express, 보안, API]
permalink: /node/express-hmac-signature/
toc: true
toc_sticky: true
date: 2024-12-28
last_modified_at: 2024-12-28
---

## HMAC 서명이란?

HMAC(Hash-based Message Authentication Code)는 암호화 해시 함수와 비밀 키를 사용하여 메시지의 무결성을 검증하고 인증을 제공하는 기술입니다. Express와 같은 Node.js 기반 서버에서 HMAC 서명을 구현하면 API 요청의 안전성과 데이터의 무결성을 보장할 수 있습니다.

---

## HMAC의 동작 방식

1. **입력 데이터**와 **비밀 키**를 결합합니다.
2. 암호화 해시 함수(SHA-256, SHA-1 등)를 사용해 해시 값을 계산합니다.
3. 생성된 HMAC 값은 메시지 인증 코드(MAC)로 사용됩니다.

이 서명 값은 클라이언트와 서버 간에 데이터 무결성을 확인하거나 요청이 승인된 클라이언트에서 온 것인지 검증하는 데 사용됩니다.

---

## HMAC 서명 구현하기 (Express 예제)

다음은 Express를 사용해 API 요청을 처리하고 HMAC 서명을 검증하는 방법입니다.

### 1. 요청 서명 생성
클라이언트가 요청에 HMAC 서명을 포함해 서버로 보낸다고 가정합니다.

```javascript
const crypto = require('crypto');

// 비밀 키 설정
const secretKey = 'my-secret-key';

// 데이터 생성
const data = JSON.stringify({ userId: 123, action: 'login' });

// HMAC 서명 생성
const hmac = crypto.createHmac('sha256', secretKey).update(data).digest('hex');

console.log('HMAC Signature:', hmac);
```

클라이언트는 생성된 `hmac` 값을 요청 헤더 또는 쿼리 파라미터로 서버에 전송합니다.

---

### 2. Express에서 HMAC 검증
서버 측에서 클라이언트가 보낸 요청의 서명을 검증합니다.

```javascript
const express = require('express');
const crypto = require('crypto');

const app = express();
const port = 3000;

// 비밀 키
const secretKey = 'my-secret-key';

// JSON 데이터 파싱
app.use(express.json());

// HMAC 검증 미들웨어
app.post('/secure-endpoint', (req, res) => {
  const receivedHmac = req.headers['x-hmac-signature']; // 클라이언트가 보낸 서명
  const data = JSON.stringify(req.body); // 요청 본문 데이터

  // HMAC 생성
  const calculatedHmac = crypto.createHmac('sha256', secretKey).update(data).digest('hex');

  // 서명 비교
  if (calculatedHmac === receivedHmac) {
    res.status(200).send('HMAC Verified: Request is valid.');
  } else {
    res.status(401).send('Invalid HMAC Signature.');
  }
});

app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
```

---

## 주요 코드 설명

- `crypto.createHmac`: HMAC 서명을 생성하는 Node.js의 내장 메서드입니다.
- `update(data)`: HMAC 계산에 사용될 데이터입니다.
- `digest('hex')`: HMAC 값을 16진수 문자열로 변환합니다.
- 클라이언트가 보낸 서명(`receivedHmac`)과 서버가 계산한 서명(`calculatedHmac`)을 비교해 요청의 무결성을 확인합니다.

---

## 보안 강화 팁

1. **HTTPS 사용**: HMAC 서명은 데이터 위변조를 방지하지만 전송 중 도청을 방지하려면 HTTPS가 필수입니다.
2. **재사용 방지**: 타임스탬프를 포함하고 일정 시간이 지난 서명은 무효화하도록 처리하세요.
3. **키 관리**: 비밀 키는 환경 변수에 저장하고 코드에 하드코딩하지 마세요.
4. **Nonce 사용**: 요청에 고유 ID를 포함해 재생 공격(replay attack)을 방지하세요.

---

HMAC 서명은 API 보안에서 널리 사용되며, 이를 통해 데이터 위변조 및 인증 문제를 효과적으로 해결할 수 있습니다. Express와 Node.js를 활용하여 안전한 API를 구현해 보세요!

