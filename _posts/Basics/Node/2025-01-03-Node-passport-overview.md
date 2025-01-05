---
title: "Node.js 인증 미들웨어 Passport: 개요와 사용법"
excerpt: "Passport를 활용하여 Node.js 애플리케이션에 다양한 인증 전략을 구현하는 방법과 코드 예제를 제공합니다."
categories:
  - Node
  - Authentication
tags:
  - [Node.js, Passport, Backend, Authentication]
permalink: /node/passport-overview/
toc: true
toc_sticky: true
date: 2025-01-03
last_modified_at: 2025-01-03
---

Node.js 애플리케이션에서 사용자 인증을 처리할 때, 강력하고 유연한 라이브러리인 `passport`를 활용하면 다양한 인증 전략을 쉽게 구현할 수 있습니다. 이 글에서는 `passport`의 주요 특징, 구성 요소, 사용법, 그리고 코드 예제를 소개합니다.

## Passport란?
`passport`는 Node.js 애플리케이션에서 인증(authentication)을 간편하게 구현할 수 있도록 설계된 미들웨어 라이브러리입니다. 다양한 인증 전략(strategy)을 지원하며, 유연성과 확장성이 뛰어납니다.

### 주요 특징
- **미들웨어 중심 설계**: Express와 같은 미들웨어 기반 프레임워크와 통합하여 동작.
- **다양한 인증 전략 지원**: 로컬(Local), OAuth, OpenID, JWT 등.
- **유연성과 확장성**: 다양한 전략을 결합하거나 맞춤 설정 가능.
- **세션 기반 인증 지원**: 세션을 통해 사용자 상태를 유지.

## Passport의 구성 요소
### 1. **Passport 미들웨어**
- `passport.initialize()`: Passport 초기화 설정.
- `passport.session()`: 세션 사용 시 세션 미들웨어와 통합.

### 2. **Strategy**
- 인증 로직을 담당하는 모듈로, 약 500개 이상의 전략 플러그인이 제공됨.

### 3. **Serialize/Deserialize**
- `serializeUser`: 인증 성공 후 사용자 정보를 세션에 저장하는 방법.
- `deserializeUser`: 세션에서 저장된 사용자 정보를 복원하는 방법.

## 설치 및 기본 설정
### Passport 설치
```bash
npm install passport
npm install passport-local express-session
```

### Express 애플리케이션에 Passport 추가
```javascript
const express = require('express');
const session = require('express-session');
const passport = require('passport');
const LocalStrategy = require('passport-local').Strategy;

const app = express();

// 사용자 데이터 예시
const users = [{ id: 1, username: 'user', password: 'pass' }];

// Passport 설정
passport.use(
  new LocalStrategy((username, password, done) => {
    const user = users.find(u => u.username === username);
    if (!user) return done(null, false, { message: '사용자를 찾을 수 없습니다.' });
    if (user.password !== password) return done(null, false, { message: '비밀번호가 틀렸습니다.' });
    return done(null, user);
  })
);

passport.serializeUser((user, done) => {
  done(null, user.id);
});

passport.deserializeUser((id, done) => {
  const user = users.find(u => u.id === id);
  done(null, user);
});

// Express 미들웨어
app.use(express.urlencoded({ extended: false }));
app.use(session({ secret: 'secret', resave: false, saveUninitialized: false }));
app.use(passport.initialize());
app.use(passport.session());

// 라우트
app.post(
  '/login',
  passport.authenticate('local', {
    successRedirect: '/success',
    failureRedirect: '/login',
  })
);

app.get('/success', (req, res) => {
  res.send(`Hello, ${req.user.username}`);
});

app.listen(3000, () => {
  console.log('Server started on http://localhost:3000');
});
```

## 주요 인증 전략
### 1. **Local Strategy**
- 사용자 이름과 비밀번호를 사용한 인증.
- 라이브러리: `passport-local`.

### 2. **JWT Strategy**
- JSON Web Token을 사용한 인증.
- 라이브러리: `passport-jwt`.

### 3. **OAuth 전략**
- Google, Facebook, GitHub 등 외부 OAuth 인증.
- 관련 라이브러리: `passport-google-oauth20`, `passport-facebook`, `passport-github` 등.

## 장점과 단점
### 장점
- 다양한 인증 전략 제공.
- 간단한 API와 모듈식 설계.
- 커스터마이즈 가능.

### 단점
- 복잡한 인증 요구사항일 경우 설정이 까다로울 수 있음.
- 세션 기반 인증은 대규모 애플리케이션에서 성능 문제가 발생할 수 있음.

`passport`는 인증에 필요한 많은 기능을 추상화하여 제공하지만, 애플리케이션에 적합한 전략을 선택하고 설정하는 것이 중요합니다. 이를 통해 효율적이고 강력한 인증 시스템을 구현할 수 있습니다.

