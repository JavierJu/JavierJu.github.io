---
title: "Node.js 환경 변수 NODE_ENV 사용법과 활용 예제"
excerpt: "Node.js 애플리케이션에서 환경 변수 NODE_ENV를 사용하여 환경별로 코드를 관리하는 방법과 활용 사례를 알아봅니다."
categories:
  - Node
  - Backend
  - Environment
  - JavaScript
  - Express
  - Tips
  
tags:
  - [Node.js, 환경 변수, NODE_ENV, Express, 서버 개발]
permalink: /node/env-variable-node-env/
toc: true
toc_sticky: true
date: 2025-01-10
last_modified_at: 2025-01-10
---

## Node.js에서 `NODE_ENV`란?
`NODE_ENV`는 Node.js 애플리케이션에서 **환경(Environment)을 구분**하기 위해 사용되는 표준 환경 변수입니다. 이를 통해 애플리케이션의 실행 환경이 **"개발(development)", "운영(production)", "테스트(test)"** 중 무엇인지 파악할 수 있습니다.

### 주요 목적
환경에 따라 **다른 설정**이나 **동작**을 구현하여 개발과 배포 과정에서 효율성과 안정성을 높이는 데 사용됩니다.

---

## 기본 문법: 조건문 활용
`NODE_ENV`는 일반적으로 다음과 같은 조건문과 함께 사용됩니다:

```javascript
if (process.env.NODE_ENV !== "production") {
    // 개발 또는 테스트 환경에서 실행되는 코드
}
```

### 코드 설명
1. **`process.env.NODE_ENV`**: 현재 Node.js 애플리케이션의 실행 환경을 나타냅니다.
2. **`!== "production"`**: `NODE_ENV` 값이 "production"이 아닌 경우를 의미합니다. 즉, **개발 환경(development)** 또는 **테스트 환경(test)**일 때 실행됩니다.

---

## `NODE_ENV`의 설정 방법
### 1. CLI에서 직접 설정
Node.js 실행 시 환경 변수를 설정합니다:
```bash
NODE_ENV=production node app.js
```
> Windows의 경우:
```bash
set NODE_ENV=production && node app.js
```

### 2. `package.json` 스크립트 활용
`package.json`에 스크립트를 추가하여 쉽게 실행 환경을 지정할 수 있습니다:
```json
"scripts": {
  "start": "NODE_ENV=production node app.js",
  "dev": "NODE_ENV=development nodemon app.js"
}
```

### 3. `.env` 파일 사용
`dotenv` 라이브러리를 사용해 `.env` 파일에서 환경 변수를 로드합니다:
```env
NODE_ENV=development
```
```javascript
if (process.env.NODE_ENV !== "production") {
    const dotenv = require("dotenv");
    dotenv.config();
}
```

---

## `NODE_ENV` 활용 사례

### 1. 개발 환경에서만 디버깅 도구 활성화
```javascript
if (process.env.NODE_ENV !== "production") {
    const morgan = require("morgan");
    app.use(morgan("dev")); // HTTP 요청 로그 미들웨어
}
```

### 2. 운영 환경과 개발 환경의 설정 분리
```javascript
const dbConfig = process.env.NODE_ENV === "production"
    ? {
        host: "prod-db-host",
        username: "prod-user",
        password: "secure-password",
      }
    : {
        host: "localhost",
        username: "dev-user",
        password: "password",
      };
```

### 3. 테스트 환경에서만 테스트 데이터 로드
```javascript
if (process.env.NODE_ENV === "test") {
    loadTestData();
}
```

---

## 왜 `NODE_ENV`를 사용하는가?
### 주요 장점
1. **환경별 코드 분리**: 환경에 따라 필요 없는 코드를 배포하지 않아 애플리케이션의 성능과 보안이 향상됩니다.
2. **코드 가독성 증가**: 조건문을 통해 실행 환경에 따른 동작을 명확히 정의할 수 있습니다.
3. **유연성 제공**: 개발, 테스트, 운영 등 다양한 환경에서 쉽게 적용 가능합니다.

---

## 결론
Node.js에서 `NODE_ENV`는 애플리케이션의 실행 환경을 구분하고, 각 환경에 맞는 설정과 코드를 실행하는 데 매우 유용합니다. 이를 잘 활용하면 개발과 배포 프로세스를 더 체계적이고 효율적으로 관리할 수 있습니다.

환경 변수 설정을 익히고 프로젝트에 적극적으로 활용해 보세요!

