---
title: "Express에서의 HTTP 쿠키: 설정, 읽기, 삭제, 그리고 보안"
excerpt: "Express에서 HTTP 쿠키를 다루는 방법과 보안 설정, 활용 사례를 다룹니다. 쿠키 설정, 읽기, 삭제와 더불어 Secure, SameSite 옵션의 중요성을 이해합니다."
categories:
  - Express
  - Node
tags:
  - [Express, Node.js, HTTP, 쿠키, 웹 개발]
permalink: /node/express-http-cookies/
toc: true
toc_sticky: true
date: 2024-12-28
last_modified_at: 2024-12-28
---

## Express에서의 HTTP 쿠키 개요

HTTP 쿠키는 클라이언트와 서버 간 상태 정보를 저장하고 전송하기 위한 중요한 메커니즘입니다. Express.js는 쿠키를 설정하거나 읽기 위해 다양한 도구를 제공합니다. 이 글에서는 Express에서 쿠키를 다루는 방법과 보안 설정에 대해 자세히 설명합니다.

---

### 1. **쿠키의 기본 개념**
- **정의**: 쿠키는 클라이언트의 웹 브라우저에 저장되는 작은 데이터 조각입니다.
- **구성 요소**:
  - **이름(name)**: 쿠키의 식별자.
  - **값(value)**: 쿠키에 저장된 데이터.
  - **옵션(options)**:
    - `maxAge`: 쿠키의 만료 시간(밀리초 단위).
    - `expires`: 쿠키의 만료 날짜(날짜 객체).
    - `httpOnly`: JavaScript에서 쿠키 접근을 방지(보안 강화).
    - `secure`: HTTPS 연결에서만 쿠키 전송.
    - `sameSite`: 크로스 사이트 요청에서 쿠키의 전송 여부 설정.

---

### 2. **Express에서 쿠키 설정 및 관리**

#### 2.1. 쿠키 설정
Express는 `res.cookie()` 메서드를 사용하여 쿠키를 설정합니다.

```javascript
const express = require('express');
const app = express();

app.get('/set-cookie', (req, res) => {
    res.cookie('username', 'JohnDoe', {
        maxAge: 900000, // 15분
        httpOnly: true, // 클라이언트의 JavaScript에서 접근 불가
        secure: true,   // HTTPS에서만 전송
        sameSite: 'strict' // 크로스 사이트 요청 차단
    });
    res.send('쿠키가 설정되었습니다.');
});

app.listen(3000, () => console.log('Server running on http://localhost:3000'));
```

#### 2.2. 쿠키 읽기
쿠키를 읽으려면 `cookie-parser` 미들웨어를 사용합니다.

**설치 및 설정**
```bash
npm install cookie-parser
```

```javascript
const express = require('express');
const cookieParser = require('cookie-parser');
const app = express();

// cookie-parser 미들웨어 사용
app.use(cookieParser());

app.get('/read-cookie', (req, res) => {
    const username = req.cookies.username; // 클라이언트의 쿠키 읽기
    if (username) {
        res.send(`안녕하세요, ${username}님!`);
    } else {
        res.send('쿠키가 없습니다.');
    }
});

app.listen(3000, () => console.log('Server running on http://localhost:3000'));
```

#### 2.3. 쿠키 삭제
`res.clearCookie()`를 사용하여 쿠키를 삭제할 수 있습니다.

```javascript
app.get('/clear-cookie', (req, res) => {
    res.clearCookie('username'); // 특정 쿠키 삭제
    res.send('쿠키가 삭제되었습니다.');
});
```

---

### 3. **Secure 및 SameSite 옵션**

- **Secure**: HTTPS 프로토콜에서만 쿠키가 전송됩니다.
- **SameSite**: 크로스 사이트 요청 위조(CSRF)를 방지합니다.
  - `Strict`: 다른 도메인에서 요청 시 쿠키가 전송되지 않음.
  - `Lax`: 사용자가 링크를 클릭한 경우에만 쿠키 전송.
  - `None`: 제한 없이 쿠키 전송(이 경우 `Secure`가 필수).

---

### 4. **JWT와 쿠키의 활용**
Express에서는 쿠키에 JSON Web Token(JWT)을 저장하여 인증에 활용할 수도 있습니다.

```javascript
const jwt = require('jsonwebtoken');
const secretKey = 'your-secret-key';

app.get('/login', (req, res) => {
    const token = jwt.sign({ username: 'JohnDoe' }, secretKey, { expiresIn: '1h' });
    res.cookie('authToken', token, { httpOnly: true, secure: true });
    res.send('로그인 성공!');
});

app.get('/verify', (req, res) => {
    const token = req.cookies.authToken;
    if (token) {
        try {
            const decoded = jwt.verify(token, secretKey);
            res.send(`인증 성공! 사용자: ${decoded.username}`);
        } catch (err) {
            res.status(401).send('유효하지 않은 토큰입니다.');
        }
    } else {
        res.status(401).send('토큰이 없습니다.');
    }
});
```

---

### 5. **쿠키의 장단점**

#### 장점
- 클라이언트에 데이터를 저장하여 서버의 부하 감소.
- 세션 관리, 사용자 식별 등 상태 정보를 유지할 수 있음.

#### 단점
- **보안 문제**:
  - 민감한 데이터는 쿠키에 저장하지 않음.
  - XSS 공격을 통해 쿠키가 노출될 위험이 있음(HTTP-only 설정 필요).
- **용량 제한**: 브라우저마다 쿠키 저장 용량 제한(일반적으로 4KB).

---

### 결론
Express에서 쿠키는 사용자 상태를 관리하고 인증/인가를 처리하는 데 유용한 도구입니다. 그러나 보안 문제를 염두에 두고 `httpOnly`, `secure`, `sameSite`와 같은 옵션을 적절히 설정하는 것이 중요합니다.

