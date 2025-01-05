---
title: "Express.js 미들웨어 분석: 세션과 사용자 정보 관리"
excerpt: "Express.js 미들웨어로 세션을 활용한 URL 리다이렉트와 사용자 정보 관리 방법을 살펴봅니다."
categories:
  - Express
  - Node
  - Middleware
tags:
  - [JavaScript, Express.js, Backend, Middleware]
permalink: /node/express-middleware-session-user-management/
toc: true
toc_sticky: true
date: 2025-01-05
last_modified_at: 2025-01-05
---

## Express.js 미들웨어 분석: 세션과 사용자 정보 관리

Express.js는 강력하고 유연한 웹 애플리케이션 프레임워크로, 미들웨어를 활용해 애플리케이션의 동작을 조정할 수 있습니다. 이 글에서는 세션과 사용자 정보를 관리하는 데 사용되는 아래 미들웨어의 코드를 분석하고, 그 작동 원리와 활용 방법을 살펴봅니다.

```javascript
app.use((req, res, next) => {
    if (!['/login', '/'].includes(req.originalUrl)) {
        req.session.returnTo = req.originalUrl;
    }
    res.locals.currentUser = req.user;

    next();
});
```

## 코드 분석

### 1. `req.session.returnTo` 설정

```javascript
if (!['/login', '/'].includes(req.originalUrl)) {
    req.session.returnTo = req.originalUrl;
}
```
- **역할**: 사용자가 로그인 페이지(`/login`) 또는 홈페이지(`/`)가 아닌 다른 URL에 접근할 경우, 해당 URL을 세션의 `returnTo` 속성에 저장합니다.
- **사용 이유**: 로그인 후 사용자를 원래 요청했던 페이지로 리다이렉트하기 위해 URL을 기억합니다.

#### 예시
1. 사용자가 `/profile`에 접근하지만 로그인되지 않은 경우 `/login` 페이지로 리다이렉트됩니다.
2. 로그인 후 `req.session.returnTo`에 저장된 `/profile`로 다시 리다이렉트됩니다.

### 2. `res.locals.currentUser` 설정

```javascript
res.locals.currentUser = req.user;
```
- **역할**: 현재 사용자 정보를 `res.locals.currentUser`에 저장합니다.
- **사용 이유**: `res.locals`는 Express의 로컬 변수 저장소로, 템플릿에서 쉽게 접근할 수 있습니다.
- **활용 예**: 템플릿 엔진(예: Pug, EJS)에서 사용자 이름, 로그인 상태 등을 표시할 때 사용합니다.

#### 템플릿 코드 예시
```html
<div>
    <p>Welcome, <%= currentUser ? currentUser.username : 'Guest' %></p>
</div>
```

### 3. `next()` 호출

```javascript
next();
```
- **역할**: 다음 미들웨어나 라우트 핸들러로 요청 처리를 넘깁니다.
- **주의점**: `next()`를 호출하지 않으면 요청 처리가 중단되므로 반드시 호출해야 합니다.

## 미들웨어 동작 흐름
1. 사용자가 특정 URL에 접근.
2. 로그인되지 않은 경우 요청 URL을 세션에 저장.
3. 로그인 페이지로 리다이렉트.
4. 로그인 성공 시 세션의 `returnTo`를 참조해 원래 요청한 페이지로 리다이렉트.

## 활용 예제

### 로그인 후 리다이렉트 구현

```javascript
app.post('/login', (req, res) => {
    // 로그인 처리 후
    const redirectTo = req.session.returnTo || '/';
    delete req.session.returnTo; // 세션에서 제거
    res.redirect(redirectTo);
});
```

### 미들웨어와 함께 동작하는 전체 흐름

```javascript
const express = require('express');
const session = require('express-session');
const app = express();

// 세션 설정
app.use(session({
    secret: 'your-secret-key',
    resave: false,
    saveUninitialized: true,
}));

// 사용자 정보와 세션 관리 미들웨어
app.use((req, res, next) => {
    if (!['/login', '/'].includes(req.originalUrl)) {
        req.session.returnTo = req.originalUrl;
    }
    res.locals.currentUser = req.user;

    next();
});

// 로그인 처리 라우트
app.post('/login', (req, res) => {
    // 로그인 로직
    req.session.user = { username: 'JohnDoe' };
    res.redirect(req.session.returnTo || '/');
});

// 로그아웃 처리
app.post('/logout', (req, res) => {
    req.session.destroy(() => {
        res.redirect('/');
    });
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

## 정리
이 미들웨어는 세션을 활용해 사용자의 요청 URL을 저장하고, 로그인 후 원래 페이지로 리다이렉트할 수 있도록 돕습니다. 또한, `res.locals`를 통해 사용자 정보를 템플릿에서 쉽게 활용할 수 있어 유용합니다. 이를 통해 Express.js 애플리케이션의 사용자 경험을 개선할 수 있습니다.

