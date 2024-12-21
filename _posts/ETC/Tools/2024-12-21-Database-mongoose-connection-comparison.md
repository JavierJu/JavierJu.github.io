---
title: "Mongoose를 사용한 MongoDB 연결 방법 비교"
excerpt: "Mongoose를 통해 MongoDB에 연결하는 두 가지 접근 방식: 이벤트 기반 처리와 Promise 기반 처리의 차이점을 알아봅니다."
categories:
  - Tools
tags:
  - [JavaScript, MongoDB, Mongoose, Backend]
permalink: /Tools/mongoose-connection-comparison/
toc: true
toc_sticky: true
date: 2024-12-21
last_modified_at: 2024-12-21
---

Mongoose는 MongoDB와의 연결을 관리하고, 데이터 모델링을 쉽게 해주는 JavaScript 라이브러리입니다. Mongoose를 사용해 MongoDB에 연결하는 방법에는 여러 가지가 있지만, 여기서는 이벤트 기반 방식과 Promise 기반 방식을 비교해 보겠습니다.

## 1. 이벤트 기반 연결 방식

```javascript
mongoose.connect('mongodb://127.0.0.1:27017/farmStand');
const db = mongoose.connection;
db.on("error", console.error.bind(console, "Database connection error"));
db.once("open", () => {
    console.log("Database connected");
});
```

### **특징:**
1. `mongoose.connection` 객체를 사용해 **이벤트 리스너**를 등록합니다.
    - `db.on("error", ...)`: 연결 중 오류를 처리합니다.
    - `db.once("open", ...)`: 연결이 성공적으로 이루어졌을 때 실행됩니다.
2. Node.js의 전통적인 이벤트 기반 접근 방식입니다.

### **장점:**
- 연결 과정에서 발생하는 다양한 이벤트(`connected`, `disconnected`, `reconnected` 등)를 세밀하게 처리할 수 있습니다.

---

## 2. Promise 기반 연결 방식

```javascript
mongoose.connect('mongodb://127.0.0.1:27017/farmStand')
    .then(() => {
        console.log("MONGO CONNECTION OPEN");
    })
    .catch(err => {
        console.log('OH NO MONGO CONNECTION ERROR!');
        console.log(err);
    });
```

### **특징:**
1. `mongoose.connect`가 반환하는 **Promise**를 사용해 연결 성공(`then`)과 오류(`catch`)를 처리합니다.
2. 최신 JavaScript에서 널리 사용되는 `Promise` 기반의 코드 스타일입니다.

### **장점:**
- 간결하고 직관적인 코드 작성이 가능합니다.
- `async/await`와 결합하면 더욱 깔끔한 비동기 흐름을 구현할 수 있습니다.

---

## 3. 두 방식 비교

|  **특징**                  | **이벤트 기반 (1번)**         | **Promise 기반 (2번)**        |
|--------------------------|-------------------------------|--------------------------------|
| **오류 처리**              | `db.on("error", ...)`         | `catch()`                     |
| **성공 처리**             | `db.once("open", ...)`        | `then()`                      |
| **가독성**                 | 상대적으로 복잡함               | 간결하고 직관적임                |
| **최신 트렌드**            | 과거 Node.js 스타일             | 최신 JavaScript 스타일           |

---

## 4. 결론

두 방식 모두 올바르게 작동하며, 상황에 따라 선택적으로 사용할 수 있습니다:

- **이벤트 기반 방식:**
  - 연결 이벤트를 세밀하게 제어하고 싶을 때 적합합니다.
  - 복잡한 연결 상태 관리가 필요한 대규모 애플리케이션에 유용합니다.

- **Promise 기반 방식:**
  - 간결한 코드를 원하거나 `async/await`를 사용하고 싶을 때 적합합니다.
  - 최신 JavaScript 스타일을 따르며, 초보자에게도 친숙합니다.

Mongoose로 MongoDB와의 연결을 설정할 때, 프로젝트의 요구 사항과 개인 선호도에 맞는 방식을 선택해 보세요!

