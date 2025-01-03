---
title: "Express에서 Session 사용 시 쿠키의 역할과 필요성"
excerpt: "Express의 session과 cookie의 관계를 이해하고, 기본적인 동작 방식과 커스터마이징 방법을 알아봅니다."
categories:
  - Node
  - Express
  - Backend
  - Web Development
tags:
  - [JavaScript, Node.js, Express, Session, Cookie]
permalink: /node/express-session-cookie-usage/
toc: true
toc_sticky: true
date: 2025-01-03
last_modified_at: 2025-01-03
---

## Express에서 Session 사용 시 쿠키의 역할과 필요성

### Session과 Cookie의 기본 개념
Express에서 `session`을 사용할 때 쿠키는 필수 요소로 작동합니다. `express-session` 라이브러리를 통해 구현되는 세션 관리는 클라이언트와 서버 간 상태를 유지하기 위한 기본적인 메커니즘을 제공합니다.

- **세션(Session)**: 서버 측에 저장되는 사용자 데이터.
- **쿠키(Cookie)**: 클라이언트 측에 저장되는 작은 데이터 조각으로, 주로 세션 ID를 전달하는 역할을 합니다.

### `express-session`의 기본 동작 원리
1. **세션 데이터 저장**
   - 서버 측에서 사용자와 관련된 데이터가 세션 스토리지(메모리, 파일, 데이터베이스 등)에 저장됩니다.
   - 서버는 각 사용자에게 고유한 세션 ID를 할당합니다.

2. **쿠키를 통한 세션 ID 전달**
   - 클라이언트에는 세션 ID가 쿠키 형태로 저장됩니다. 기본적으로 `connect.sid`라는 키가 사용됩니다.
   - 클라이언트가 서버로 요청을 보낼 때 이 쿠키를 함께 전송하면, 서버는 이를 통해 세션 데이터를 조회할 수 있습니다.

### Express에서 Session 설정 예제
다음은 `express-session`을 사용한 세션 설정의 기본 코드 예제입니다:

```javascript
const express = require('express');
const session = require('express-session');

const app = express();

app.use(session({
  secret: 'your-secret-key', // 세션 암호화 키
  resave: false, // 세션이 수정되지 않아도 저장 여부
  saveUninitialized: true, // 초기화되지 않은 세션 저장 여부
  cookie: {
    secure: true, // HTTPS에서만 전송
    httpOnly: true, // JavaScript로 쿠키 접근 방지
    maxAge: 60000 // 쿠키 유효 시간 (밀리초 단위)
  }
}));

app.get('/', (req, res) => {
  if (!req.session.views) {
    req.session.views = 1;
  } else {
    req.session.views++;
  }
  res.send(`You visited this page ${req.session.views} times`);
});

app.listen(3000, () => console.log('Server running on http://localhost:3000'));
```

### 쿠키를 별도로 관리해야 하는 경우
기본적으로 `express-session`이 세션 관리에 필요한 쿠키를 자동으로 생성하고 관리하지만, 특정 상황에서는 쿠키를 별도로 설정하거나 관리할 필요가 있습니다.

#### 1. **쿠키 설정 커스터마이징**
보안 요구 사항이나 만료 시간 설정을 변경하고자 할 때, `cookie` 옵션을 조정할 수 있습니다:

```javascript
app.use(session({
  secret: 'your-secret-key',
  resave: false,
  saveUninitialized: true,
  cookie: {
    secure: true, // HTTPS에서만 전송
    httpOnly: true, // JavaScript로 쿠키 접근 불가
    maxAge: 3600000 // 1시간 동안 유효
  }
}));
```

#### 2. **세션 외 데이터 저장**
클라이언트 측에 세션 외의 데이터를 저장하려면 별도의 쿠키를 설정해야 할 수도 있습니다:

```javascript
app.get('/set-cookie', (req, res) => {
  res.cookie('customKey', 'customValue', { maxAge: 3600000, httpOnly: true });
  res.send('Custom cookie has been set');
});
```

#### 3. **JWT와 같은 인증 방식 사용**
JWT(JSON Web Token)와 같은 별도의 인증 방식에서는 쿠키를 이용해 토큰을 저장하거나, 헤더를 통해 전달할 수 있습니다. 이 경우 세션을 사용하지 않고도 클라이언트 상태를 유지할 수 있습니다.

### 요약
- **필요 없는 경우**: 기본적인 세션 관리에는 `express-session`이 쿠키를 자동으로 처리하므로, 별도의 쿠키 관리는 필요하지 않습니다.
- **필요한 경우**: 보안 강화, 세션 외 데이터 저장, 혹은 커스터마이징이 필요할 경우 쿠키 설정을 조정해야 합니다.

Express와 함께 세션과 쿠키를 올바르게 설정하면, 사용자 인증 및 데이터 관리를 효율적으로 수행할 수 있습니다.

