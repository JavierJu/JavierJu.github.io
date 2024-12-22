---
title: "Express의 미들웨어: 동작 원리와 활용 방법"
excerpt: "Express의 미들웨어 개념, 종류, 사용법, 그리고 실제 사례를 통해 웹 애플리케이션 개발의 핵심 요소를 살펴봅니다."
categories:
  - Node
tags:
  - [Express, Middleware, Node.js, Web Development]
permalink: /Node/express-middleware-guide/
toc: true
toc_sticky: true
date: 2024-12-22
last_modified_at: 2024-12-22
---

## Express의 미들웨어란?

Express는 Node.js 환경에서 실행되는 인기 있는 웹 애플리케이션 프레임워크로, 미들웨어는 Express의 핵심 개념 중 하나입니다. 미들웨어를 이해하고 효과적으로 활용하면 Express 애플리케이션의 구조를 깔끔하고 확장 가능하게 설계할 수 있습니다.

---

### 1. 미들웨어의 정의
미들웨어(Middleware)는 요청(request)과 응답(response) 객체를 처리하거나, 요청과 응답 사이의 작업을 수행하기 위해 Express 애플리케이션에서 실행되는 함수입니다.

#### 특징:
- 요청-응답 주기 동안 실행됩니다.
- `next()`를 호출하여 다음 미들웨어로 넘어가거나, 응답을 종료할 수 있습니다.
- 체인처럼 연결되어 요청 처리 파이프라인을 구성합니다.

---

### 2. 미들웨어의 형식
미들웨어 함수는 다음과 같은 형식을 따릅니다:

```javascript
function middleware(req, res, next) {
    // 요청(req) 또는 응답(res)을 처리하거나,
    // 다음 미들웨어로 넘어가도록 next() 호출
}
```

#### 매개변수:
1. `req`: 클라이언트의 요청 객체.
2. `res`: 서버의 응답 객체.
3. `next`: 다음 미들웨어를 호출하는 콜백 함수.

---

### 3. 미들웨어의 종류
미들웨어는 크게 다음 세 가지로 분류됩니다:

#### 1. 애플리케이션 레벨 미들웨어
- `app.use()` 또는 `app.METHOD()`로 정의됩니다.
- 특정 경로나 모든 경로에서 실행 가능합니다.

**예제:**
```javascript
const express = require('express');
const app = express();

// 애플리케이션 레벨 미들웨어
app.use((req, res, next) => {
    console.log('Request URL:', req.originalUrl);
    next();
});

app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.listen(3000);
```

#### 2. 라우터 레벨 미들웨어
- `express.Router()` 인스턴스에 연결된 미들웨어입니다.
- 특정 라우트에서만 동작합니다.

**예제:**
```javascript
const express = require('express');
const router = express.Router();

// 라우터 레벨 미들웨어
router.use((req, res, next) => {
    console.log('Router Middleware');
    next();
});

router.get('/user', (req, res) => {
    res.send('User Route');
});

const app = express();
app.use('/api', router);
app.listen(3000);
```

#### 3. 빌트인 미들웨어
- Express에 기본적으로 포함된 미들웨어입니다.
- 예: `express.json()`, `express.urlencoded()`, `express.static()`

**예제:**
```javascript
const express = require('express');
const app = express();

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

app.post('/data', (req, res) => {
    res.json(req.body);
});

app.listen(3000);
```

#### 4. 서드파티 미들웨어
- npm 패키지로 제공되는 미들웨어입니다.
- 예: `morgan`, `body-parser`, `cors`

**예제:**
```javascript
const express = require('express');
const morgan = require('morgan');

const app = express();

// 서드파티 미들웨어
app.use(morgan('tiny'));

app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.listen(3000);
```

---

### 4. 미들웨어 동작 원리
1. 요청이 들어오면, Express는 등록된 미들웨어를 순서대로 실행합니다.
2. 각 미들웨어는 요청을 처리하거나, `next()`를 호출하여 다음 미들웨어로 전달합니다.
3. 요청-응답 사이클을 끝내려면 `res.send()` 또는 `res.json()` 등 응답 메서드를 호출합니다.

**예제:**
```javascript
app.use((req, res, next) => {
    console.log('Middleware 1');
    next(); // 다음 미들웨어로 넘어감
});

app.use((req, res, next) => {
    console.log('Middleware 2');
    res.send('Response from Middleware 2'); // 응답 종료
});

app.use((req, res, next) => {
    console.log('Middleware 3'); // 실행되지 않음
    next();
});
```

---

### 5. 미들웨어 활용 사례
1. **요청 데이터 처리:**
   - JSON 및 URL-encoded 데이터 파싱.
2. **로깅 및 디버깅:**
   - 요청 정보를 기록.
   - 예: `morgan`.
3. **보안:**
   - 인증 및 권한 부여.
   - 예: `passport`, `helmet`.
4. **정적 파일 제공:**
   - `express.static`을 사용하여 정적 파일 제공.
5. **에러 처리:**
   - 에러를 처리하고 응답하는 미들웨어.

---

### 6. 에러 처리 미들웨어
에러 처리 미들웨어는 다른 미들웨어와 비슷하지만, 매개변수로 에러 객체(`err`)를 추가로 받습니다.

**예제:**
```javascript
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).send('Something broke!');
});
```

---

### 7. 실행 순서 관리
- 미들웨어는 등록된 순서대로 실행됩니다.
- 순서를 조정하여 요청 처리 흐름을 설계합니다.

**예제:**
```javascript
app.use((req, res, next) => {
    console.log('First Middleware');
    next();
});

app.get('/', (req, res) => {
    console.log('Route Handler');
    res.send('Hello World!');
});
```

---

### 8. 정리
Express의 미들웨어는 요청-응답 사이의 모든 로직을 처리할 수 있는 강력한 도구입니다. 미들웨어를 효과적으로 사용하면 유지보수 가능한 모듈식 코드를 작성할 수 있습니다. 필요한 예제나 특정 질문이 있다면 추가로 알려주세요! 😊

