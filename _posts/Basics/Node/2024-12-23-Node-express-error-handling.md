---
title: "Express에서 앱의 오류를 처리하는 방법"
excerpt: "Express에서 애플리케이션의 오류를 효과적으로 처리하는 방법을 알아보고, 에러 핸들링 미들웨어 설정 및 비동기 에러 관리 팁을 제공합니다."
categories:
  - Node
tags:
  - [JavaScript, Express, Backend, 오류 처리]
permalink: /Node/express-error-handling/
toc: true
toc_sticky: true
date: 2024-12-23
last_modified_at: 2024-12-23
---

Express에서 애플리케이션의 오류를 처리하는 방법은 중요하며, 이를 통해 사용자 경험을 개선하고 보안을 강화할 수 있습니다. Express는 기본적으로 에러 핸들링을 지원하지만, 개발자가 커스터마이징하여 적절히 처리해야 합니다. 아래에 단계별로 오류 처리 방법을 자세히 설명하겠습니다.

---

## 1. 기본 에러 핸들링

Express에서는 미들웨어로 에러 핸들링을 구현할 수 있습니다. 에러 핸들링 미들웨어는 일반 미들웨어와 비슷하지만, `err` 인수를 추가로 받습니다.

```javascript
app.use((err, req, res, next) => {
  console.error(err.stack); // 에러 로그 출력
  res.status(500).send('Something broke!');
});
```

---

## 2. 전역 에러 핸들링

애플리케이션 전체에서 발생할 수 있는 에러를 중앙 집중적으로 처리하려면, 다음과 같이 전역 에러 핸들러를 설정합니다:

### 기본 사용

```javascript
app.use((err, req, res, next) => {
  res.status(err.status || 500);
  res.json({
    error: {
      message: err.message,
    },
  });
});
```

### 사용자 정의 에러 객체 활용

```javascript
class AppError extends Error {
  constructor(message, status) {
    super(message);
    this.status = status;
  }
}

// 예외 발생 시
app.get('/', (req, res, next) => {
  next(new AppError('Route not found', 404));
});
```

---

## 3. 404 에러 처리

존재하지 않는 경로로 요청이 들어왔을 때를 처리하기 위해, 별도의 404 핸들러를 설정할 수 있습니다.

```javascript
app.use((req, res, next) => {
  res.status(404).send('Sorry, we cannot find that!');
});
```

---

## 4. 비동기 에러 처리

Express 5 이전 버전에서는 비동기 코드에서 발생한 에러를 `next()`로 전달해야 합니다.

### 예제:

```javascript
app.get('/async-route', async (req, res, next) => {
  try {
    const data = await someAsyncFunction();
    res.json(data);
  } catch (err) {
    next(err);
  }
});
```

Express 5부터는 비동기 함수에서 던진 에러를 자동으로 처리합니다.

```javascript
app.get('/async-route', async (req, res) => {
  const data = await someAsyncFunction(); // 에러 발생 시 자동 전달
  res.json(data);
});
```

---

## 5. 환경별 에러 메시지

개발 환경과 프로덕션 환경에서 다른 에러 메시지를 보여주는 것이 중요합니다.

```javascript
app.use((err, req, res, next) => {
  const isDevelopment = process.env.NODE_ENV === 'development';

  res.status(err.status || 500);
  res.json({
    message: isDevelopment ? err.message : 'Internal Server Error',
    ...(isDevelopment && { stack: err.stack }),
  });
});
```

---

## 6. 로그 관리

에러 로그를 콘솔에 출력하는 대신 파일, 데이터베이스, 혹은 클라우드 서비스(Sentry 등)에 저장할 수 있습니다.

### 예제: `morgan`을 사용한 로그 기록

```javascript
const morgan = require('morgan');
app.use(morgan('combined'));
```

### 예제: Sentry 통합

```javascript
const Sentry = require('@sentry/node');
Sentry.init({ dsn: 'https://example.sentry.io/your-dsn' });

app.use(Sentry.Handlers.requestHandler());

// 에러 발생 시 Sentry에 보고
app.use(Sentry.Handlers.errorHandler());
```

---

## 7. 모든 에러 잡기

Express 외부에서 발생한 에러를 처리하기 위해 `process` 이벤트를 사용할 수 있습니다.

```javascript
process.on('uncaughtException', (err) => {
  console.error('Uncaught Exception:', err);
  process.exit(1); // 필요 시 프로세스 종료
});

process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Rejection at:', promise, 'reason:', reason);
});
```

---

## 8. 최종 예제

아래는 다양한 에러 처리 방식을 종합한 Express 애플리케이션의 예제입니다:

```javascript
const express = require('express');
const app = express();

// 미들웨어
app.use(express.json());

// 정상 라우트
app.get('/', (req, res) => {
  res.send('Hello World!');
});

// 비정상 라우트
app.get('/error', (req, res, next) => {
  next(new Error('Something went wrong!'));
});

// 404 핸들러
app.use((req, res, next) => {
  res.status(404).send('Route not found');
});

// 에러 핸들러
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.status || 500).json({
    message: err.message,
  });
});

// 서버 시작
app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

---

위와 같이 설정하면 다양한 유형의 오류를 효과적으로 처리하고, 사용자와 개발자 모두에게 유용한 환경을 제공합니다. 추가적인 질문이 있다면 댓글로 알려주세요! 😊

