---
title: "Express에서 쿠키 파서 미들웨어(cookie-parser) 자세히 알아보기"
excerpt: "Express.js에서 cookie-parser를 사용하여 쿠키를 처리하고 관리하는 방법을 예제와 함께 알아봅니다."
categories:
  - Node
  - Express
  - Backend
  - Web Development
tags:
  - [JavaScript, Express.js, 쿠키, Backend, 미들웨어]
permalink: /node/express-cookie-parser/
toc: true
toc_sticky: true
date: 2024-12-28
last_modified_at: 2024-12-28
---

`Express.js`에서 쿠키 파서는 클라이언트에서 전송된 HTTP 요청 헤더에 포함된 쿠키를 해석하고, 이를 JavaScript 객체로 변환하여 쉽게 사용할 수 있게 하는 미들웨어입니다. 이를 통해 애플리케이션에서 쿠키 기반 세션 관리, 사용자 인증, 사용자 설정 저장 등을 구현할 수 있습니다.

## cookie-parser 설치

`cookie-parser`는 Express에서 쿠키를 파싱하기 위해 사용하는 인기 있는 미들웨어입니다. 먼저, 아래 명령어를 사용해 `cookie-parser`를 설치합니다.

```bash
npm install cookie-parser
```

## 예제 코드

### 1. 기본 사용법

```javascript
const express = require('express');
const cookieParser = require('cookie-parser');

const app = express();

// 쿠키 파서 미들웨어 등록
app.use(cookieParser());

// 라우트 설정
app.get('/', (req, res) => {
  // 클라이언트가 전송한 쿠키를 로그로 출력
  console.log('Cookies:', req.cookies);

  // 응답에 새로운 쿠키 설정
  res.cookie('username', 'JohnDoe', { maxAge: 900000, httpOnly: true });
  res.send('Cookie has been set');
});

// 서버 시작
app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

#### 설명
1. `cookie-parser` 미들웨어를 Express 앱에 등록합니다.
2. `req.cookies` 객체를 통해 클라이언트로부터 전송된 쿠키를 확인할 수 있습니다.
3. `res.cookie` 메서드를 사용하여 클라이언트에 쿠키를 설정합니다.

### 2. 서명된 쿠키 사용

쿠키에 서명을 추가하면, 서버가 쿠키의 무결성을 보장할 수 있습니다. 이를 위해 `cookie-parser`는 비밀키를 지원합니다.

```javascript
const express = require('express');
const cookieParser = require('cookie-parser');

const app = express();

// 쿠키 파서 미들웨어에 서명용 비밀키 전달
app.use(cookieParser('mySecretKey'));

// 라우트 설정
app.get('/', (req, res) => {
  // 서명된 쿠키 설정
  res.cookie('secureData', '12345', { signed: true, httpOnly: true });

  // 서명된 쿠키 확인
  console.log('Signed Cookies:', req.signedCookies);

  res.send('Signed cookie has been set');
});

app.get('/check', (req, res) => {
  // 서명된 쿠키 확인
  if (req.signedCookies.secureData === '12345') {
    res.send('Signed cookie is valid');
  } else {
    res.send('Signed cookie is invalid or missing');
  }
});

// 서버 시작
app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

#### 설명
1. `cookieParser('mySecretKey')`와 같이 비밀키를 전달하여 서명된 쿠키를 활성화합니다.
2. `res.cookie`를 사용해 쿠키를 생성할 때 `{ signed: true }` 옵션을 추가합니다.
3. 클라이언트가 전송한 서명된 쿠키는 `req.signedCookies` 객체에서 확인할 수 있습니다.

---

## 주요 옵션

1. **`maxAge`**: 쿠키의 만료 시간을 밀리초 단위로 설정합니다.
2. **`httpOnly`**: 클라이언트 측에서 JavaScript를 통해 쿠키에 접근하지 못하도록 설정합니다. (보안 강화)
3. **`secure`**: HTTPS 연결에서만 쿠키를 전송하도록 설정합니다.
4. **`signed`**: 쿠키를 서명하여 무결성을 보장합니다.

---

## 쿠키 삭제

쿠키를 삭제하려면 `res.clearCookie` 메서드를 사용합니다.

```javascript
app.get('/logout', (req, res) => {
  res.clearCookie('username');
  res.send('Cookie has been cleared');
});
```

---

Express.js에서 `cookie-parser`는 쿠키와 관련된 작업을 간단하게 처리할 수 있도록 돕는 유용한 미들웨어입니다. 위 내용을 참고하여 애플리케이션에서 효율적으로 쿠키를 관리해보세요!

