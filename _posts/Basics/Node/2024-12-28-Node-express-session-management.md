---
title: "Express의 세션 관리: 구조, 설정, 그리고 보안 고려사항"
excerpt: "Express에서의 세션 관리 동작 방식과 설정, 보안 최적화 방안을 자세히 설명합니다."
categories:
  - Node
  - Express
tags:
  - [Node.js, Express, Backend, 세션 관리, 웹 개발]
permalink: /node/express-session-management/
toc: true
toc_sticky: true
date: 2024-12-28
last_modified_at: 2024-12-28
---

## Express에서의 세션 관리란?

Express에서 세션(session)은 사용자의 상태 정보를 서버에 저장하고 클라이언트와 서버 간의 상태를 유지하기 위해 사용하는 중요한 기능입니다. 로그인 상태, 장바구니 정보, 사용자 설정 등과 같은 데이터를 저장하는 데 주로 사용됩니다. 

Express에서 세션 관리는 **`express-session`** 미들웨어를 사용하여 구현되며, 세션 동작 방식, 구성 요소, 설정 및 보안에 대해 아래에서 자세히 설명합니다.

---

## 세션의 동작 방식

1. **클라이언트와 서버의 초기 요청:**
   - 사용자가 웹사이트를 방문하면 서버는 고유한 세션 ID를 생성합니다.
   - 이 세션 ID는 클라이언트의 쿠키에 저장됩니다.

2. **세션 데이터 저장:**
   - 세션 ID와 연관된 데이터는 서버에 저장됩니다.
   - 세션 저장소(storage)에 데이터를 저장하며, 이는 기본적으로 메모리, 파일, 데이터베이스(redis, MongoDB 등)를 사용할 수 있습니다.

3. **후속 요청에서 세션 식별:**
   - 클라이언트는 요청마다 세션 ID를 쿠키를 통해 서버에 전송합니다.
   - 서버는 세션 ID를 기반으로 저장된 데이터를 조회하여 클라이언트를 식별하고 상태를 유지합니다.

---

## 주요 구성 요소

1. **세션 ID:**
   - 고유한 식별자로, 클라이언트와 서버 간 세션을 연결하는 역할을 합니다.
   - 무작위 문자열로 생성되며, 암호화되어 전송됩니다.

2. **쿠키:**
   - 세션 ID를 클라이언트 측에 저장합니다.
   - 기본적으로 `connect.sid`라는 이름으로 쿠키가 생성됩니다.

3. **세션 저장소:**
   - 세션 데이터가 저장되는 공간으로, 메모리, 파일, 데이터베이스(Redis, MongoDB 등)를 사용할 수 있습니다.

---

## `express-session` 설정 및 사용 방법

### 1. 설치

```bash
npm install express-session
```

### 2. 기본 설정

```javascript
const express = require('express');
const session = require('express-session');

const app = express();

app.use(
  session({
    secret: 'mySecretKey', // 세션 암호화를 위한 키
    resave: false,         // 세션이 변경되지 않아도 저장 여부
    saveUninitialized: true, // 초기화되지 않은 세션도 저장 여부
    cookie: {
      secure: false,       // HTTPS 환경에서만 쿠키 사용 여부
      httpOnly: true,      // 클라이언트에서 JavaScript로 쿠키 접근 방지
      maxAge: 60000,       // 쿠키 만료 시간 (1분)
    },
  })
);

app.get('/', (req, res) => {
  if (!req.session.views) {
    req.session.views = 1;
  } else {
    req.session.views++;
  }
  res.send(`Number of views: ${req.session.views}`);
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

### 3. 주요 옵션 설명

- **`secret`:** 세션 데이터의 무결성을 보호하기 위한 키. 복잡한 문자열을 사용.
- **`resave`:** 세션 데이터가 변경되지 않아도 세션을 다시 저장할지 여부.
- **`saveUninitialized`:** 초기화되지 않은 세션을 저장할지 여부.
- **`cookie`:** 세션 쿠키의 속성을 정의.
  - `secure`: HTTPS 환경에서만 쿠키가 전송되도록 설정.
  - `httpOnly`: JavaScript를 통해 쿠키를 접근하지 못하도록 설정.
  - `maxAge`: 쿠키의 만료 시간을 설정.

### 4. 세션 데이터 추가 및 삭제

```javascript
app.get('/set', (req, res) => {
  req.session.username = 'JohnDoe';
  res.send('Session data set');
});

app.get('/get', (req, res) => {
  const username = req.session.username;
  res.send(`Hello, ${username || 'Guest'}`);
});

app.get('/destroy', (req, res) => {
  req.session.destroy((err) => {
    if (err) {
      return res.status(500).send('Failed to destroy session');
    }
    res.clearCookie('connect.sid'); // 쿠키 삭제
    res.send('Session destroyed');
  });
});
```

---

## 세션 저장소 변경 (Redis 예시)

### 1. 설치

```bash
npm install connect-redis redis
```

### 2. 설정

```javascript
const RedisStore = require('connect-redis')(session);
const redis = require('redis');

const redisClient = redis.createClient();

app.use(
  session({
    store: new RedisStore({ client: redisClient }),
    secret: 'mySecretKey',
    resave: false,
    saveUninitialized: false,
    cookie: { secure: false, maxAge: 60000 },
  })
);
```

---

## 세션 보안 고려사항

1. **HTTPS 및 `secure` 옵션 사용:**
   - `cookie.secure` 옵션을 활성화하고 HTTPS를 사용하여 쿠키를 안전하게 전송.

2. **`httpOnly` 설정:**
   - 쿠키가 JavaScript를 통해 접근되지 않도록 설정하여 XSS 공격 방지.

3. **세션 고정 방지:**
   - 로그인 시 새로운 세션을 생성하여 세션 고정 공격 방지.

4. **세션 만료 설정:**
   - 오래된 세션이 남아있지 않도록 `maxAge`를 설정.

5. **외부 저장소 사용:**
   - 메모리 저장소는 프로덕션 환경에 적합하지 않으므로 Redis 또는 데이터베이스 사용.

---

Express에서 세션 관리는 사용자 상태를 유지하는 데 매우 유용하며, 올바른 설정과 보안 조치를 통해 안정적이고 확장성 있는 애플리케이션을 개발할 수 있습니다.
