---
title: "Express에서 커스텀 에러 클래스 활용하기: AppError 설명과 사용법"
excerpt: "Express 애플리케이션에서 에러를 체계적으로 처리하기 위한 AppError 클래스의 구현과 활용법을 알아봅니다."
categories:
  - Node
  
tags:
  - [JavaScript, Node.js, Express, Backend, Error Handling]
permalink: /Node/express-app-error-handling/
toc: true
toc_sticky: true
date: 2024-12-23
last_modified_at: 2024-12-23
---

## Express에서의 커스텀 에러 처리
Express는 널리 사용되는 Node.js 웹 프레임워크로, 에러 처리 미들웨어를 통해 애플리케이션의 안정성과 유지 보수성을 높일 수 있습니다. 여기서는 커스텀 에러 클래스 `AppError`를 구현하고 이를 통해 에러를 체계적으로 관리하는 방법을 알아보겠습니다.

### AppError 클래스의 구현
다음은 커스텀 에러 클래스를 정의하는 코드입니다.

```javascript
class AppError extends Error {
    constructor(message, status) {
        super();
        this.message = message;
        this.status = status;
    }
}

module.exports = AppError;
```

#### 코드 설명
1. **`class AppError extends Error`**
   - `Error` 객체를 상속받아 새로운 기능을 추가합니다.
   - `Error`는 JavaScript의 기본 내장 객체로, 오류를 처리하는 데 사용됩니다.

2. **`constructor` 메서드**
   - 에러 메시지와 HTTP 상태 코드를 받아 이를 인스턴스 속성으로 설정합니다.
   - `super()`는 부모 클래스의 생성자를 호출합니다. 이를 통해 기본 `Error` 객체의 기능을 초기화합니다.

3. **`message`와 `status`**
   - `message`: 에러 메시지 문자열을 저장합니다.
   - `status`: HTTP 상태 코드를 저장하여 적절한 응답을 클라이언트에 보낼 수 있도록 합니다.

### 사용 예시

#### 1. 에러 객체 생성
`AppError`를 사용하여 에러 객체를 생성할 수 있습니다.

```javascript
const AppError = require('./AppError');

const error = new AppError("Resource not found", 404);
console.log(error.message); // "Resource not found"
console.log(error.status);  // 404
```

#### 2. Express 애플리케이션에서 활용
`AppError` 클래스는 Express의 미들웨어와 함께 사용하면 더욱 유용합니다.

```javascript
const express = require('express');
const AppError = require('./AppError');

const app = express();

// 라우트 예시
app.get('/example', (req, res, next) => {
    if (!req.query.name) {
        // 조건에 맞지 않으면 커스텀 에러를 발생시킵니다.
        throw new AppError("Name query parameter is required", 400);
    }
    res.send("Hello, " + req.query.name);
});

// 에러 처리 미들웨어
app.use((err, req, res, next) => {
    const status = err.status || 500; // 상태 코드가 없으면 기본값 500
    const message = err.message || "Something went wrong";
    res.status(status).json({ error: message });
});

// 서버 시작
app.listen(3000, () => {
    console.log("Server is running on port 3000");
});
```

#### 작동 원리
1. 특정 조건에서 `AppError` 객체를 생성하고 `throw`로 에러를 발생시킵니다.
2. Express는 에러가 발생하면 에러 처리 미들웨어로 요청을 전달합니다.
3. 에러 처리 미들웨어는 `AppError` 객체의 `status`와 `message`를 참조해 적절한 HTTP 응답을 생성합니다.

### AppError의 장점

1. **일관성 있는 에러 처리**
   - 모든 에러에 대해 동일한 속성 구조(`message`, `status`)를 가지므로 처리 로직이 간단해집니다.

2. **가독성 향상**
   - HTTP 상태 코드와 메시지를 명확히 정의하여 코드의 의도를 더 잘 전달할 수 있습니다.

3. **확장 가능성**
   - 필요에 따라 `AppError`를 확장하여 더 구체적인 에러 클래스를 정의할 수 있습니다.

#### 확장 예시
```javascript
class ValidationError extends AppError {
    constructor(message, fields) {
        super(message, 422);
        this.fields = fields; // 유효성 검사가 실패한 필드 정보
    }
}
```

### 결론
`AppError` 클래스는 Express 애플리케이션에서 체계적이고 효율적인 에러 처리 방식을 구현하는 데 도움을 줍니다. 이를 활용하면 코드의 유지 보수성과 확장성을 크게 향상시킬 수 있습니다. 이제 커스텀 에러 클래스를 사용하여 더욱 안정적인 애플리케이션을 구축해 보세요!

