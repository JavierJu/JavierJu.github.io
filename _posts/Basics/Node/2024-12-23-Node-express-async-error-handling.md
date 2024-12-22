---
title: "Express에서 비동기 오류 처리하기: 안전하고 효율적인 방법"
excerpt: "Express 애플리케이션에서 비동기 코드의 오류를 안전하게 처리하는 방법과 Best Practices를 알아봅니다."
categories:
  - Node
tags:
  - [Express, Node.js, Backend, 비동기 처리, 오류 처리]
permalink: /Node/express-async-error-handling/
toc: true
toc_sticky: true
date: 2024-12-23
last_modified_at: 2024-12-23
---

Express에서 비동기 오류를 처리하는 것은 중요하며, 적절한 방식으로 처리하지 않으면 오류가 누락되거나 서버가 멈출 수 있습니다. 아래는 Express에서 비동기 오류를 처리하는 방법과 원리를 자세히 설명합니다.

---

## 1. **비동기 코드에서 오류가 발생하는 이유**
Express는 기본적으로 동기적인 미들웨어와 라우트 핸들러에서 발생하는 오류를 처리하도록 설계되었습니다. 하지만 비동기 코드(예: `async/await` 또는 `Promise`)에서 발생한 오류는 기본적인 `try-catch` 없이 자동으로 Express의 오류 처리 미들웨어로 전달되지 않습니다.

---

## 2. **전통적인 방법: try-catch 사용**

비동기 핸들러에서 `try-catch`를 사용하여 오류를 잡고, `next()`를 호출해 Express의 오류 처리 미들웨어로 전달할 수 있습니다.

```javascript
const express = require('express');
const app = express();

app.get('/async-route', async (req, res, next) => {
    try {
        const result = await someAsyncFunction();
        res.json(result);
    } catch (err) {
        next(err); // 오류를 Express의 오류 처리 미들웨어로 전달
    }
});

// 오류 처리 미들웨어
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ error: 'Something went wrong!' });
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

---

## 3. **반복적인 try-catch를 줄이는 방법**

### (1) **비동기 유틸리티 함수 작성**
공통적인 패턴을 줄이기 위해 비동기 핸들러를 감싸는 유틸리티 함수를 사용할 수 있습니다.

```javascript
const asyncHandler = (fn) => (req, res, next) => {
    Promise.resolve(fn(req, res, next)).catch(next);
};

// 사용 예시
app.get('/async-route', asyncHandler(async (req, res) => {
    const result = await someAsyncFunction();
    res.json(result);
}));
```

이 방식은 중복되는 `try-catch`를 제거하고, 가독성을 높여줍니다.

---

### (2) **외부 라이브러리 활용**
[express-async-errors](https://www.npmjs.com/package/express-async-errors) 라이브러리는 비동기 핸들러에서 발생한 오류를 자동으로 Express의 오류 처리 미들웨어로 전달합니다.

#### 설치
```bash
npm install express-async-errors
```

#### 사용법
```javascript
require('express-async-errors');
const express = require('express');
const app = express();

app.get('/async-route', async (req, res) => {
    const result = await someAsyncFunction();
    res.json(result);
});

// 오류 처리 미들웨어
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ error: 'Something went wrong!' });
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

`express-async-errors`를 사용하면 추가적인 코드 없이 비동기 오류를 처리할 수 있습니다.

---

## 4. **오류 처리 미들웨어**
Express에서 오류 처리 미들웨어는 반드시 네 가지 인자를 가져야 합니다.

```javascript
app.use((err, req, res, next) => {
    console.error(err.stack); // 로그 출력
    res.status(err.status || 500).json({ error: err.message || 'Internal Server Error' });
});
```

### 주요 특징
- `err` 객체는 비동기 핸들러에서 `next(err)`로 전달된 오류를 포함합니다.
- `status` 속성이 정의된 경우, 이를 사용하여 상태 코드를 설정합니다.

---

## 5. **전역 처리**
Express에서 처리되지 않은 모든 오류를 마지막으로 처리하려면 전역 오류 처리기를 정의해야 합니다.

### 예제
```javascript
process.on('unhandledRejection', (reason, promise) => {
    console.error('Unhandled Rejection:', reason);
    // 로그 기록 또는 알림 전송 등
});

process.on('uncaughtException', (err) => {
    console.error('Uncaught Exception:', err);
    // 로그 기록 또는 서버 종료 등
    process.exit(1); // 필요에 따라 프로세스 종료
});
```

---

## 6. **Best Practices**
- 가능한 경우 `asyncHandler` 또는 `express-async-errors`와 같은 도구를 사용하여 오류 처리를 단순화합니다.
- 공통적인 오류 형식을 정의하고, 클라이언트에 의미 있는 응답을 반환하도록 합니다.
- 로그와 모니터링 시스템을 통해 오류를 추적하고 분석합니다.

위의 방법을 사용하면 Express 애플리케이션에서 비동기 코드의 오류를 안전하고 효과적으로 처리할 수 있습니다.

