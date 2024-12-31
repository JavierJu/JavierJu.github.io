---
title: "Express 앱에서의 인증: 세션, JWT, OAuth 구현 방법"
excerpt: "Express.js 애플리케이션에서 사용자 인증을 구현하는 세션, JWT, OAuth 방식과 보안 고려사항에 대해 알아봅니다."
categories:
  - Node
  - Express
  - Backend
  - Authentication
tags:
  - [Express.js, Node.js, 인증, Backend, OAuth, JWT, 세션]
permalink: /node/express-authentication-guide/
toc: true
toc_sticky: true
date: 2024-12-31
last_modified_at: 2024-12-31
---

## Express 앱에서의 인증 개요

Express.js는 가볍고 유연한 웹 프레임워크로, 다양한 인증 방식과 기술을 쉽게 통합할 수 있습니다. 이 글에서는 세션 기반 인증, JSON Web Token(JWT)을 사용한 토큰 기반 인증, 그리고 OAuth와 같은 외부 인증을 사용하는 방법에 대해 설명합니다.

---

## 1. 인증 방식 선택
Express 앱에서 사용할 수 있는 주요 인증 방식은 다음과 같습니다:

### 1.1 세션 기반 인증
- **작동 원리**:
  - 사용자가 로그인하면 서버가 세션을 생성하고, 클라이언트는 세션 ID를 쿠키에 저장합니다.
  - 서버는 요청에서 쿠키를 확인하여 세션 데이터를 인증합니다.
- **장점**:
  - 서버가 세션 데이터를 관리하므로 보안성이 높습니다.
- **단점**:
  - 서버에 추가적인 저장소가 필요하며 확장성이 떨어질 수 있습니다.
- **구현 방법**:
  ```javascript
  const session = require('express-session');

  app.use(session({
    secret: 'your-secret-key',
    resave: false,
    saveUninitialized: true,
    cookie: { secure: true } // HTTPS에서만 사용
  }));
  ```

### 1.2 토큰 기반 인증 (JWT)
- **작동 원리**:
  - 서버에서 JWT를 생성하여 클라이언트에 전달합니다.
  - 클라이언트는 JWT를 요청 헤더에 포함하여 인증을 처리합니다.
- **장점**:
  - 서버 상태를 유지하지 않아 확장성이 뛰어납니다.
- **단점**:
  - 토큰 유효 기간 관리가 필요하며, 탈취 위험이 있습니다.
- **구현 방법**:
  ```javascript
  const jwt = require('jsonwebtoken');

  // 토큰 생성
  const token = jwt.sign({ userId: 123 }, 'your-secret-key', { expiresIn: '1h' });

  // 토큰 검증 미들웨어
  const verifyToken = (req, res, next) => {
    const token = req.headers['authorization'];
    if (!token) return res.status(403).send('No token provided.');
    jwt.verify(token, 'your-secret-key', (err, decoded) => {
      if (err) return res.status(500).send('Failed to authenticate token.');
      req.userId = decoded.userId;
      next();
    });
  };

  app.get('/protected', verifyToken, (req, res) => {
    res.send('This is a protected route.');
  });
  ```

### 1.3 OAuth 인증
- **작동 원리**:
  - 외부 제공자(Google, Facebook 등)를 통해 인증 과정을 수행.
- **장점**:
  - 인증 과정을 외부 제공자에게 위임하여 보안성과 편의성을 제공합니다.
- **단점**:
  - 설정 및 통합 과정이 복잡할 수 있습니다.
- **구현 방법** (Passport.js 사용):
  ```javascript
  const passport = require('passport');
  const GoogleStrategy = require('passport-google-oauth20').Strategy;

  passport.use(new GoogleStrategy({
    clientID: 'YOUR_CLIENT_ID',
    clientSecret: 'YOUR_CLIENT_SECRET',
    callbackURL: 'http://localhost:3000/auth/google/callback'
  }, (accessToken, refreshToken, profile, done) => {
    // 사용자 데이터 처리
    return done(null, profile);
  }));

  app.use(passport.initialize());

  app.get('/auth/google', passport.authenticate('google', { scope: ['profile'] }));

  app.get('/auth/google/callback', passport.authenticate('google', { failureRedirect: '/' }), (req, res) => {
    res.redirect('/protected');
  });
  ```

---

## 2. 보안 고려사항
인증 구현 시 다음 사항을 고려하여 보안을 강화해야 합니다:
1. **HTTPS 사용**: 데이터를 암호화하여 안전하게 전송.
2. **CORS 설정**: 허용된 도메인에서만 요청 가능하도록 설정.
3. **CSRF 방지**: CSRF 토큰 사용 또는 `SameSite` 쿠키 속성을 설정.
4. **세션 관리**: `HttpOnly` 및 `Secure` 쿠키 플래그 설정.
5. **토큰 저장**: JWT는 `HttpOnly` 쿠키에 저장하여 XSS 공격을 방지.

---

## 3. 인증 라이브러리 추천
1. **Passport.js**: 다양한 인증 전략을 지원하는 미들웨어.
2. **express-session**: 세션 기반 인증 구현을 위한 라이브러리.
3. **jsonwebtoken (JWT)**: 토큰 생성 및 검증을 위한 라이브러리.
4. **bcrypt**: 비밀번호 해싱 및 검증.

---

Express.js에서 요구 사항에 맞는 인증 방식을 선택하고, 위의 방법을 참고하여 구현해 보세요. 질문이 있다면 댓글로 남겨주세요!

