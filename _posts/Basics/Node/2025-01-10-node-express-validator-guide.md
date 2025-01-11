---
title: "Express-validator: Node.js와 Express에서의 입력 유효성 검사 완벽 가이드"
excerpt: "Express-validator를 활용한 입력 데이터 검증과 정규화 방법을 알아보고, 안전한 Node.js 애플리케이션을 구축하는 방법을 소개합니다."
categories:
  - Node
  - Express
  - Validation
tags:
  - [Node.js, Express, Validation, Backend]
permalink: /node/express-validator-guide/
toc: true
toc_sticky: true
date: 2025-01-10
last_modified_at: 2025-01-10
---

`express-validator`는 **Node.js** 및 **Express** 애플리케이션에서 입력 유효성 검사를 간편하게 수행할 수 있도록 도와주는 라이브러리입니다. 이 라이브러리는 미들웨어로 작동하며, 클라이언트로부터 전달받은 데이터가 특정 기준을 만족하는지 검사하고, 문제 있는 데이터를 식별하여 처리할 수 있도록 도와줍니다.

## 주요 기능

1. **체크 및 유효성 검사 (Validation)**  
   클라이언트에서 보내는 데이터가 특정 조건을 만족하는지 확인합니다. 예를 들어, 이메일 형식 확인, 숫자 범위 제한, 문자열 길이 확인 등이 포함됩니다.

2. **데이터 정규화 (Sanitization)**  
   입력값을 정리하여 안전하게 처리할 수 있도록 변환합니다. 예를 들어, 문자열 양쪽 공백 제거, HTML 특수문자 이스케이프, 소문자 변환 등이 가능합니다.

3. **체인 방식 (Chainable API)**  
   메서드를 체인 형태로 연결하여 간결하고 가독성 높은 코드를 작성할 수 있습니다.

4. **사용자 정의 유효성 검사 (Custom Validation)**  
   필요에 따라 특정한 로직을 포함한 커스텀 유효성 검사를 정의할 수 있습니다.

---

## 설치 및 기본 사용법

### 설치
```bash
npm install express-validator
```

### 기본 예제
```javascript
const express = require('express');
const { body, validationResult } = require('express-validator');

const app = express();
app.use(express.json()); // JSON 요청 파싱

app.post('/user',
  // 유효성 검사 규칙
  [
    body('username').isLength({ min: 5 }).withMessage('사용자 이름은 최소 5자 이상이어야 합니다.'),
    body('email').isEmail().withMessage('유효한 이메일 주소를 입력하세요.'),
    body('password').isLength({ min: 8 }).withMessage('비밀번호는 최소 8자 이상이어야 합니다.'),
  ],
  (req, res) => {
    const errors = validationResult(req);

    if (!errors.isEmpty()) {
      // 유효성 검사 실패 시 에러 반환
      return res.status(400).json({ errors: errors.array() });
    }

    // 유효성 검사 통과 시 처리
    res.send('사용자 등록 성공!');
  }
);

app.listen(3000, () => console.log('서버가 http://localhost:3000에서 실행 중입니다.'));
```

---

## 주요 메서드

### 1. **유효성 검사 메서드**

| 메서드                | 설명                                           |
|-----------------------|-----------------------------------------------|
| `isEmail()`           | 유효한 이메일 주소인지 확인                   |
| `isLength({ min, max })` | 문자열 길이가 지정된 범위인지 확인           |
| `isNumeric()`         | 숫자인지 확인                                 |
| `isAlphanumeric()`    | 알파벳 및 숫자만 포함되어 있는지 확인         |
| `isBoolean()`         | 불리언 값인지 확인                           |
| `isDate()`            | 날짜 형식인지 확인                           |

### 2. **정규화 메서드**

| 메서드                   | 설명                                       |
|--------------------------|-------------------------------------------|
| `trim()`                 | 문자열 양쪽의 공백 제거                   |
| `escape()`               | HTML 특수문자를 이스케이프                |
| `toLowerCase()`          | 문자열을 소문자로 변환                    |
| `toUpperCase()`          | 문자열을 대문자로 변환                    |
| `toInt()`                | 문자열을 정수로 변환                      |

---

## 유용한 패턴

### 1. **커스텀 유효성 검사**
```javascript
body('username').custom(value => {
  if (value.includes('admin')) {
    throw new Error('사용자 이름에 "admin"을 포함할 수 없습니다.');
  }
  return true;
});
```

### 2. **비동기 유효성 검사**
```javascript
body('email').custom(async value => {
  const existingUser = await User.findOne({ email: value });
  if (existingUser) {
    throw new Error('이미 등록된 이메일입니다.');
  }
});
```

### 3. **조건부 검사**
```javascript
body('phone')
  .if(body('contactPreference').equals('phone'))
  .isMobilePhone().withMessage('유효한 전화번호를 입력하세요.');
```

---

`express-validator`는 강력하고 유연한 입력 유효성 검사 도구로, Express 애플리케이션의 보안과 신뢰성을 높이는 데 크게 기여합니다. 필요한 규칙에 따라 조합하여 사용할 수 있으며, 유효성 검사 실패 시 명확한 에러 메시지를 반환하여 디버깅과 사용자 피드백에 유용합니다.
