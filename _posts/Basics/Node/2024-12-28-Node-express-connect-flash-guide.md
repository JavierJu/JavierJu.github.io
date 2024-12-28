---
title: "Express.js에서 connect-flash 활용법: 일시적인 메시지 전달의 필수 도구"
excerpt: "connect-flash의 개념, 설치, 사용법, 그리고 실용적인 예제를 통해 Express.js 애플리케이션에서 효과적으로 플래시 메시지를 처리하는 방법을 배워보세요."
categories:
  - Express
  - Node
tags:
  - [JavaScript, Express.js, Backend, 플래시 메시지]
permalink: /node/express-connect-flash-guide/
toc: true
toc_sticky: true
date: 2024-12-28
last_modified_at: 2024-12-28
---

## connect-flash란?

`connect-flash`는 Express.js 애플리케이션에서 사용자에게 일시적인 메시지를 전달하는 데 사용되는 미들웨어입니다. 일반적으로 성공 메시지, 오류 메시지 또는 상태 알림과 같은 데이터를 사용자에게 전달하기 위해 사용됩니다. 이 패키지는 세션(`express-session`)을 기반으로 동작하며, 플래시 메시지가 한 번 표시된 후 자동으로 삭제되도록 설계되었습니다.

---

## 주요 특징

1. **일시적인 메시지 저장**  
   플래시 메시지는 한 번 읽히면 세션에서 삭제됩니다.
2. **세션 기반 동작**  
   메시지를 유지하려면 `express-session`이 필요합니다.
3. **간단한 API**  
   메시지를 설정 및 읽기가 쉽습니다.

---

## 설치

`connect-flash`를 사용하려면 먼저 설치해야 합니다.

```bash
npm install connect-flash
```

---

## 사용 방법

### 1. Express 애플리케이션에 연결

```javascript
const express = require('express');
const session = require('express-session');
const flash = require('connect-flash');

const app = express();

// 세션 설정
app.use(session({
  secret: 'your-secret-key',
  resave: false,
  saveUninitialized: true,
}));

// 플래시 미들웨어 등록
app.use(flash());

// 전역 변수로 플래시 메시지 전달
app.use((req, res, next) => {
  res.locals.successMessage = req.flash('success');
  res.locals.errorMessage = req.flash('error');
  next();
});

// 라우트 예제
app.get('/success', (req, res) => {
  req.flash('success', 'This is a success message!');
  res.redirect('/');
});

app.get('/', (req, res) => {
  res.send(`
    <h1>Home</h1>
    <p>${res.locals.successMessage || ''}</p>
    <p>${res.locals.errorMessage || ''}</p>
  `);
});

app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

---

### 2. 주요 API

#### `req.flash(key, value)`
- **설명**: 특정 키에 메시지를 저장합니다.
- **매개변수**:
  - `key`: 메시지의 키 (예: 'success', 'error')
  - `value`: 저장할 메시지 (문자열 또는 배열)
- **예제**:
  ```javascript
  req.flash('info', 'This is an informational message');
  ```

#### `req.flash(key)`
- **설명**: 특정 키의 메시지를 가져옵니다. 메시지는 가져온 후 자동으로 삭제됩니다.
- **매개변수**:
  - `key`: 가져올 메시지의 키
- **반환값**: 메시지 배열
- **예제**:
  ```javascript
  const messages = req.flash('info');
  console.log(messages); // ['This is an informational message']
  ```

---

## 실용적인 사용 예제

### 1. 사용자 인증
로그인 실패 시 오류 메시지를 전달하는 경우:
```javascript
app.post('/login', (req, res) => {
  const { username, password } = req.body;

  if (username !== 'admin' || password !== 'password') {
    req.flash('error', 'Invalid username or password');
    return res.redirect('/login');
  }

  req.flash('success', 'Logged in successfully');
  res.redirect('/');
});
```

### 2. 회원 가입
회원 가입 완료 후 성공 메시지 표시:
```javascript
app.post('/register', (req, res) => {
  // 회원 가입 로직
  req.flash('success', 'Registration successful! You can now log in.');
  res.redirect('/login');
});
```

---

## 참고 사항

1. **의존성**  
   `connect-flash`는 세션에 의존하므로 반드시 `express-session`을 설정해야 합니다.
2. **플래시 메시지의 제한**  
   플래시 메시지는 임시 저장 용도로만 사용됩니다. 데이터를 영구적으로 유지하려면 데이터베이스 등을 사용해야 합니다.
3. **템플릿 엔진 통합**  
   템플릿 엔진(e.g., EJS, Pug)을 사용하는 경우, 플래시 메시지를 뷰에 쉽게 렌더링할 수 있습니다.

---

위 내용을 참고하여 `connect-flash`를 효과적으로 활용해 보세요!

