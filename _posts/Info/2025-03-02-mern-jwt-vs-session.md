---
title: "MERN 스택에서 JWT 인증과 express-session 인증 비교"
excerpt: "MERN 스택에서 JWT 기반 인증 방식과 express-session 기반 인증 방식의 차이점을 정리하고, 각각의 장단점과 사용 사례를 코드 예제와 함께 설명합니다."
categories:
  - MERN
  - Backend
  - Authentication
  - Web Development
  - Info
tags:
  - [MERN, JWT, express-session, 인증, 보안]
permalink: /mern/jwt-vs-session/
toc: true
toc_sticky: true
date: 2025-03-02
last_modified_at: 2025-03-02
---

# MERN 스택에서 JWT 인증과 express-session 인증 비교

MERN 스택에서 로그인 및 인증을 구현할 때 주로 사용하는 두 가지 방식인 **JWT 기반 인증**과 **express-session 기반 인증**의 차이점을 비교해보겠습니다.

## 1. JWT 기반 인증 (JSON Web Token)

### 🔹 개요

JWT 기반 인증은 클라이언트가 로그인하면 서버에서 JWT를 발급하고, 이후 요청마다 JWT를 헤더에 포함시켜 인증을 수행하는 방식입니다. 토큰은 **stateless**하기 때문에 서버는 세션을 저장할 필요가 없습니다.

### 🔹 동작 방식

```javascript
const jwt = require("jsonwebtoken");
const secretKey = "your_secret_key";

// 로그인 시 JWT 생성
app.post("/login", (req, res) => {
  const { username } = req.body;
  const token = jwt.sign({ username }, secretKey, { expiresIn: "1h" });
  res.json({ token });
});

// JWT 검증 미들웨어
const verifyToken = (req, res, next) => {
  const token = req.headers["authorization"];
  if (!token) return res.status(403).json({ message: "토큰이 필요합니다." });
  jwt.verify(token, secretKey, (err, decoded) => {
    if (err)
      return res.status(401).json({ message: "토큰이 유효하지 않습니다." });
    req.user = decoded;
    next();
  });
};
```

### 🔹 장점과 단점

✅ **Stateless (상태 비저장)** → 서버 부담 감소, 확장성 증가  
✅ **다양한 클라이언트에서 사용 가능** → 모바일 앱, 웹 앱, 마이크로서비스에 적합  
❌ **토큰 유출 시 보안 위험** → 클라이언트가 직접 관리해야 함  
❌ **로그아웃 처리 어려움** → 블랙리스트 DB 필요

---

## 2. express-session 기반 인증

### 🔹 개요

express-session 기반 인증은 서버에서 사용자 정보를 세션에 저장하고, 클라이언트는 `sessionID`를 쿠키로 저장하는 방식입니다.

### 🔹 동작 방식

```javascript
const session = require("express-session");

app.use(
  session({
    secret: "your_secret_key",
    resave: false,
    saveUninitialized: true,
    cookie: { secure: false }, // HTTPS 환경에서는 true로 설정
  })
);

// 로그인 시 세션 생성
app.post("/login", (req, res) => {
  req.session.user = { username: req.body.username };
  res.json({ message: "로그인 성공" });
});

// 인증 미들웨어
const isAuthenticated = (req, res, next) => {
  if (req.session.user) {
    next();
  } else {
    res.status(401).json({ message: "로그인이 필요합니다." });
  }
};
```

### 🔹 장점과 단점

✅ **보안성 (서버에서 세션 관리)** → 유출 위험 적음  
✅ **로그아웃 처리 쉬움** → 서버에서 세션 삭제 가능  
❌ **서버 메모리 사용 증가** → 많은 사용자가 접속하면 부담  
❌ **확장성 문제** → Redis 같은 세션 저장소 필요

---

## 🔥 JWT vs express-session 비교 정리

|                   | **JWT (JSON Web Token)**          | **express-session**             |
| ----------------- | --------------------------------- | ------------------------------- |
| **저장 위치**     | 클라이언트 (로컬스토리지, 쿠키)   | 서버 (메모리, DB)               |
| **서버 부하**     | 낮음 (stateless)                  | 높음 (stateful)                 |
| **확장성**        | 높음 (수평 확장 가능)             | 낮음 (서버 간 세션 공유 필요)   |
| **로그아웃 처리** | 어려움 (토큰 유효기간까지 유지됨) | 쉬움 (서버에서 세션 삭제 가능)  |
| **보안성**        | 클라이언트에서 토큰을 관리해야 함 | 서버에서 세션을 관리            |
| **사용 사례**     | 모바일, 마이크로서비스, API 인증  | 웹 애플리케이션, 단일 서버 환경 |

---

## ✅ 어떤 걸 선택해야 할까?

- **SPA (Single Page Application) + REST API** 개발 → ✅ **JWT 추천**
- **일반적인 웹 애플리케이션 (SSR, 로그인 유지 필요)** → ✅ **express-session 추천**
- **OAuth2 같은 토큰 기반 인증이 필요하다면** → ✅ **JWT**

### 🔹 추가 고려사항

- **보안 강화**: JWT는 **HttpOnly 쿠키**로 저장하는 게 안전함.
- **로그아웃 구현**: JWT를 사용할 경우, **Redis로 블랙리스트 관리** 가능.
- **확장성 고려**: 세션을 사용할 경우, **Redis 같은 세션 저장소** 필요.

---

**결론:**  
✅ **SPA + API 백엔드** 개발 → **JWT 사용**  
✅ **SSR 기반 웹앱 또는 단일 서버 환경** → **express-session 사용**  
✅ **OAuth2 같은 토큰 기반 인증이 필요하다면** → **JWT**
