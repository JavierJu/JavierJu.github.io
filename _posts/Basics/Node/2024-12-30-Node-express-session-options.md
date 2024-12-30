---
title: "Express 세션 옵션 상세 가이드: 세션 관리의 모든 것"
excerpt: "Express에서 세션을 관리하기 위한 다양한 옵션과 그 동작 방식을 상세히 설명합니다. 자주 사용되는 설정을 포함하여 세션 최적화를 위한 팁을 제공합니다."
categories:
  - Node
  - Express
  - Backend
  - Web Development
tags:
  - [Node.js, Express, Backend, 세션 관리, 웹 개발]
permalink: /node/express-session-options/
toc: true
toc_sticky: true
date: 2024-12-30
last_modified_at: 2024-12-30
---

## Express 세션 옵션 상세 설명

Express에서 세션 관리 시 사용되는 주요 옵션과 그 동작 방식을 상세히 알아봅니다. 기본적으로 `express-session` 미들웨어에서 설정 가능한 옵션들로, 이들을 적절히 활용하면 효율적이고 안전한 세션 관리를 구현할 수 있습니다.

---

### 1. `secret`
- **설명**: 세션 데이터를 암호화하기 위한 키입니다.
- **역할**: 세션 ID의 무결성을 보호하고, 클라이언트가 세션 ID를 위조하지 못하도록 방지합니다.
- **설정 예시**:
  ```javascript
  secret: 'mySuperSecretKey'
  ```
- **추가 팁**:
  - 보안 강화를 위해 랜덤 문자열 생성기를 사용하여 키를 생성하세요.
  - 다중 키 사용 시:
    ```javascript
    secret: ['key1', 'key2', 'key3']
    ```

---

### 2. `resave`
- **설명**: 세션 데이터가 변경되지 않아도 요청마다 세션을 저장할지 여부를 설정합니다.
- **옵션 값**:
  - `true`: 세션이 항상 저장됩니다.
  - `false`: 세션이 변경된 경우에만 저장됩니다.
- **권장 설정**: 
  - `false`로 설정하여 불필요한 저장소와의 통신을 방지하고 성능을 향상시킵니다.
- **설정 예시**:
  ```javascript
  resave: false
  ```

---

### 3. `saveUninitialized`
- **설명**: 초기화되지 않은 세션(데이터가 없는 세션)을 저장할지 여부를 설정합니다.
- **옵션 값**:
  - `true`: 초기 세션도 저장합니다.
  - `false`: 데이터가 없는 세션은 저장하지 않습니다.
- **권장 설정**:
  - `false`로 설정하여 사용자가 동의하지 않은 상태에서 세션을 생성하지 않도록 합니다.
- **설정 예시**:
  ```javascript
  saveUninitialized: false
  ```

---

### 4. `cookie`
- **설명**: 세션 쿠키의 속성을 설정하는 객체입니다.
- **세부 옵션**:
  1. **`secure`**:
     - HTTPS 환경에서만 쿠키를 전송하도록 설정합니다.
     - **설정**:
       ```javascript
       cookie: { secure: true }
       ```
  2. **`httpOnly`**:
     - 클라이언트 측 JavaScript로 쿠키를 접근하지 못하도록 방지합니다.
     - **설정**:
       ```javascript
       cookie: { httpOnly: true }
       ```
  3. **`maxAge`**:
     - 쿠키의 만료 시간을 밀리초 단위로 설정합니다.
     - **설정**:
       ```javascript
       cookie: { maxAge: 60000 } // 1분
       ```
  4. **`sameSite`**:
     - 쿠키의 사이트 간 요청 동작을 설정합니다.
       - `Strict`: 동일 사이트에서만 쿠키가 전송됩니다.
       - `Lax`: 일부 크로스 사이트 요청(GET)에서만 전송됩니다.
       - `None`: 모든 요청에 쿠키가 포함됩니다(HTTPS 필요).
     - **설정**:
       ```javascript
       cookie: { sameSite: 'strict' }
       ```
  5. **`path`**:
     - 쿠키가 전송되는 경로를 설정합니다.
     - 기본값은 `/`입니다.

---

### 자주 사용되는 기타 옵션

#### 1. `store`
- **설명**: 세션 데이터를 저장할 저장소를 지정합니다.
- **기본값**: 메모리 저장소(비추천)
- **추천 저장소**:
  - Redis, MongoDB, MySQL 등.
- **설정 예시**:
  ```javascript
  const RedisStore = require('connect-redis')(session);
  const redisClient = require('redis').createClient();

  app.use(
    session({
      store: new RedisStore({ client: redisClient }),
      secret: 'mySecretKey',
      resave: false,
      saveUninitialized: false,
    })
  );
  ```

---

#### 2. `rolling`
- **설명**: 사용자의 요청이 있을 때마다 세션 쿠키의 만료 시간을 갱신할지 여부를 설정합니다.
- **옵션 값**:
  - `true`: 요청마다 쿠키의 만료 시간을 갱신합니다.
  - `false`: 고정된 만료 시간을 유지합니다.
- **설정 예시**:
  ```javascript
  rolling: true
  ```

---

#### 3. `unset`
- **설명**: 세션이 삭제될 때 `req.session`의 상태를 정의합니다.
- **옵션 값**:
  - `'destroy'`: 세션 삭제 시 `req.session`을 `null`로 설정.
  - `'keep'`: 세션 데이터를 삭제하지만 객체는 유지.
- **설정 예시**:
  ```javascript
  unset: 'destroy'
  ```

---

#### 4. `proxy`
- **설명**: 서버가 프록시 뒤에 있는 경우 쿠키의 보안을 설정합니다.
- **옵션 값**:
  - `true`: `secure` 쿠키가 올바르게 동작하도록 설정.
- **설정 예시**:
  ```javascript
  app.set('trust proxy', 1);
  ```

---

## 종합 설정 예시

```javascript
const session = require('express-session');
const RedisStore = require('connect-redis')(session);
const redisClient = require('redis').createClient();

app.use(
  session({
    secret: 'mySuperSecretKey',
    resave: false,
    saveUninitialized: false,
    rolling: true,
    unset: 'destroy',
    store: new RedisStore({ client: redisClient }),
    cookie: {
      secure: process.env.NODE_ENV === 'production',
      httpOnly: true,
      maxAge: 60000,
      sameSite: 'strict',
    },
  })
);
```

---

### 요약
- **필수 옵션**: `secret`, `cookie`, `store` (프로덕션 환경에서는 반드시 설정 필요)
- **유용한 추가 옵션**: `rolling`, `unset`, `proxy`

Express 세션 옵션을 적절히 설정하면 보안과 성능을 극대화할 수 있습니다. 세부 설정에 따라 애플리케이션의 요구사항에 맞춘 최적화가 가능합니다.
