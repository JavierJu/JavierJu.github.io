---
title: "Express에서 404 처리: app.use와 app.all의 차이"
excerpt: "Express에서 404 오류를 처리할 때 app.use와 app.all('*')의 차이점과 각각의 사용 사례를 비교하고, 최적의 선택을 안내합니다."
categories:
  - Node
tags:
  - [JavaScript, Express, Backend, 오류 처리, 404]
permalink: /Node/express-404-handling/
toc: true
toc_sticky: true
date: 2024-12-24
last_modified_at: 2024-12-24
---

Express에서 애플리케이션의 404 오류를 처리하는 방법은 중요하며, 사용자의 요청이 실패했을 때 명확하고 친절한 응답을 제공할 수 있도록 도와줍니다. 주로 사용되는 두 가지 방법인 `app.use`와 `app.all('*')`의 차이와 장단점을 아래에서 자세히 살펴보겠습니다.

---

## 1. `app.use`를 사용한 404 처리

```javascript
app.use((req, res, next) => {
    res.status(404).send('Sorry, we cannot find that!');
});
```

### 특징
- **모든 HTTP 메서드**(GET, POST, PUT, DELETE 등)에 대해 매칭됩니다.
- 일반적으로 **미들웨어의 마지막에 위치**하며, 이전에 정의된 라우트 핸들러에서 처리되지 않은 요청에 대해 실행됩니다.
- **기본적인 404 처리 방식**으로 많이 사용됩니다.

### 장점
- 코드가 간단하며, 미들웨어의 흐름에 자연스럽게 통합됩니다.

### 단점
- 명시적으로 경로를 지정하지 않기 때문에 코드 가독성이 약간 떨어질 수 있습니다.
- 다른 미들웨어 이후에 적절히 배치되지 않으면 의도하지 않은 동작을 초래할 수 있습니다.

---

## 2. `app.all('*')`를 사용한 404 처리

```javascript
app.all('*', (req, res, next) => {
    res.status(404).send('Sorry, we cannot find that!');
});
```

### 특징
- `app.all`은 **모든 HTTP 메서드**에 대해 특정 경로에 매칭됩니다.
- `'*'` 경로는 모든 요청 경로를 포함하므로, **명시적으로 모든 라우트에 대한 404 처리를 정의**합니다.
- 일반적으로 **미들웨어 체인의 마지막**에 위치합니다.

### 장점
- 명시적으로 모든 경로를 처리하므로 코드가 더 직관적이고 가독성이 높습니다.
- 대규모 애플리케이션에서 더 나은 유지보수성을 제공합니다.

### 단점
- 단순한 애플리케이션에서는 약간의 오버헤드처럼 보일 수 있습니다.

---

## 3. 두 방법의 비교

| 특성                  | `app.use`                                   | `app.all('*')`                             |
|-----------------------|---------------------------------------------|-------------------------------------------|
| **호출 시점**         | 미들웨어 체인의 마지막                     | 미들웨어 체인의 마지막                     |
| **명시성**            | 낮음                                       | 높음                                       |
| **코드 가독성**       | 상대적으로 덜 직관적                       | 명확하고 직관적                             |
| **주요 사용 사례**    | 소규모 애플리케이션                       | 대규모 애플리케이션                         |

---

## 4. 추천 사용 가이드

### `app.use`를 사용하는 경우:
- 간단한 애플리케이션에서 빠르고 쉽게 404 처리를 추가하고 싶을 때.
- 미들웨어 흐름의 일부로 404 처리를 통합하려는 경우.

### `app.all('*')`를 사용하는 경우:
- 애플리케이션의 **라우트 처리를 명시적으로 정의**하고 싶을 때.
- 대규모 프로젝트에서 코드의 가독성과 유지보수를 중요시하는 경우.

---

## 5. 최종 예제

아래는 두 방식을 결합하여 사용하는 완전한 Express 애플리케이션의 예제입니다:

```javascript
const express = require('express');
const app = express();

// 정규 라우트 핸들러
app.get('/', (req, res) => {
    res.send('Welcome to the homepage!');
});

// 다른 라우트
app.get('/about', (req, res) => {
    res.send('About us page');
});

// 404 처리: app.all('*') 사용
app.all('*', (req, res) => {
    res.status(404).send('Sorry, we cannot find that!');
});

// 서버 시작
app.listen(3000, () => {
    console.log('Server is running on port 3000');
});
```

---

### 결론
- **작은 프로젝트**에서는 `app.use`를 사용하는 것이 간단하고 충분합니다.
- **대규모 프로젝트**에서는 `app.all('*')`를 통해 명시적이고 직관적인 404 처리를 구현하는 것이 더 좋습니다.

Express에서 404를 처리하는 방법을 이해하고, 프로젝트의 규모와 요구 사항에 맞는 방식을 선택하세요! 😊

