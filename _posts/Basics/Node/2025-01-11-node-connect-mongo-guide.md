---
title: "Connect-Mongo: Express와 MongoDB로 세션 관리하기"
excerpt: "Connect-Mongo를 활용한 Express 애플리케이션의 세션 관리 방법과 사용 사례를 알아봅니다. MongoDB 기반의 세션 저장소 설정과 주요 옵션을 소개합니다."
categories:
  - Node
  - MongoDB
tags:
  - [Express, MongoDB, Backend, Node.js, 세션 관리]
permalink: /node/connect-mongo-guide/
toc: true
toc_sticky: true
date: 2025-01-11
last_modified_at: 2025-01-11
---

## Connect-Mongo란?

`connect-mongo`는 **Express** 애플리케이션에서 세션 데이터를 MongoDB에 저장하기 위해 사용되는 미들웨어입니다. 이를 통해 서버를 재시작해도 세션 데이터가 유지되며, 클러스터 환경에서도 세션 상태를 공유할 수 있습니다.

---

## 주요 특징

1. **MongoDB 기반 세션 저장소**
   - 세션 데이터를 MongoDB에 영구적으로 저장할 수 있습니다.

2. **자동 만료 (TTL)**
   - 세션 만료 시간을 설정하여 오래된 세션 데이터를 자동으로 삭제합니다.

3. **클러스터 및 분산 환경 지원**
   - 중앙화된 세션 관리를 통해 여러 서버 간 상태를 공유할 수 있습니다.

4. **유연한 설정**
   - MongoDB 연결 URI, 콜렉션 이름, TTL 등 다양한 옵션을 제공합니다.

---

## 설치 방법

```bash
npm install connect-mongo
```

---

## 기본 사용법

`express-session`과 함께 사용하여 MongoDB를 세션 저장소로 설정할 수 있습니다.

### 코드 예시

```javascript
const express = require('express');
const session = require('express-session');
const MongoStore = require('connect-mongo');

const app = express();

// MongoDB 연결 URI
const mongoURI = 'mongodb://localhost:27017/myDatabase';

app.use(
  session({
    secret: 'mySecret', // 세션 암호화 키
    resave: false, // 세션이 수정되지 않은 경우에도 다시 저장할지 여부
    saveUninitialized: false, // 초기화되지 않은 세션을 저장할지 여부
    store: MongoStore.create({
      mongoUrl: mongoURI, // MongoDB 연결 URI
      collectionName: 'sessions', // 세션 데이터를 저장할 컬렉션 이름
    }),
    cookie: {
      maxAge: 1000 * 60 * 60 * 24, // 1일 (밀리초 단위)
    },
  })
);

app.get('/', (req, res) => {
  if (req.session.views) {
    req.session.views++;
    res.send(`Number of views: ${req.session.views}`);
  } else {
    req.session.views = 1;
    res.send('Welcome! Refresh the page.');
  }
});

app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

---

## 주요 옵션

1. **`mongoUrl`**
   - MongoDB 연결 문자열 (필수).
   - 예: `'mongodb://localhost:27017/myDatabase'`.

2. **`collectionName`**
   - 세션 데이터를 저장할 MongoDB 컬렉션 이름 (기본값: `'sessions'`).

3. **`ttl`**
   - 세션 만료 시간 (초 단위, 기본값: `14 * 24 * 60 * 60` = 14일).
   - 예: `ttl: 60 * 60 * 24` (1일).

4. **`mongoOptions`**
   - MongoDB 클라이언트의 추가 옵션.
   - 예: `{ useNewUrlParser: true, useUnifiedTopology: true }`.

5. **`touchAfter`**
   - 세션이 변경되지 않은 경우에도 마지막 접속 시간을 업데이트할 주기 (초 단위).
   - 예: `touchAfter: 24 * 3600` (하루에 한 번만 업데이트).

---

## 장점

1. **데이터 영구 저장**
   - 서버가 재시작되더라도 세션 데이터가 유지됩니다.

2. **확장성**
   - 여러 서버 또는 클러스터 환경에서 세션 공유가 가능합니다.

3. **자동화된 세션 관리**
   - TTL 설정으로 오래된 세션 데이터를 MongoDB가 자동으로 삭제합니다.

---

## 주의 사항

1. **MongoDB가 실행 중이어야 함**
   - `connect-mongo`를 사용하려면 MongoDB 서버가 실행 중이어야 합니다.

2. **적절한 TTL 설정 필요**
   - 세션 데이터의 만료 시간(`ttl`)을 적절히 설정하여 MongoDB에 불필요한 데이터가 쌓이지 않도록 관리해야 합니다.

3. **보안**
   - 세션 데이터를 안전하게 관리하기 위해 강력한 `secret` 값을 설정하세요.
   - HTTPS 환경에서 `secure: true` 쿠키 설정을 사용하는 것이 좋습니다.

---

`connect-mongo`를 활용하면 Express 애플리케이션에서 안정적이고 확장 가능한 세션 관리를 구현할 수 있습니다. 추가로 궁금한 점이 있다면 댓글로 질문해 주세요! 😊

