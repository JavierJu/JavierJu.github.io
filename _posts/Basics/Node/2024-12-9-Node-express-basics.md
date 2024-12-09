---
title: "Express.js 기초: 요청, 응답, 라우팅, 경로 매개변수와 쿼리 문자열"
excerpt: "Express.js를 활용한 요청 및 응답 객체, 기본 라우팅, 경로 매개변수, 쿼리 문자열 처리에 대한 기초를 다룹니다."
categories:
  - Node
tags:
  - [JavaScript, Express.js, Backend, 웹 개발]
permalink: /Node/express-basics/
toc: true
toc_sticky: true
date: 2024-12-9
last_modified_at: 2024-12-9
---

## Express의 기초

### 1. **Express란?**
Express는 Node.js 환경에서 실행되는 경량화된 웹 프레임워크로, 웹 애플리케이션 및 API를 쉽게 구축할 수 있도록 돕습니다. 간단한 라우팅, 요청 및 응답 관리, 미들웨어 활용 등이 주요 특징입니다.

---

### 2. **요청 및 응답 객체**

`req`와 `res`는 각각 요청(request)과 응답(response) 객체를 나타냅니다. 

#### 예시:
```js
app.get('/', (req, res) => {
    res.send('Hello, Express!');
});
```
- `req`: 클라이언트 요청과 관련된 모든 데이터를 담고 있는 객체입니다.
- `res`: 서버가 클라이언트에 응답을 보낼 때 사용하는 객체입니다.
  - `res.send()`: 텍스트나 HTML 등을 클라이언트에 보냅니다.
  - `res.json()`: JSON 형식으로 데이터를 응답합니다.

---

### 3. **라우팅**

라우팅은 특정 URL 경로에 클라이언트 요청을 처리하는 방식을 정의합니다.

#### 예시:
```js
app.get('/', (req, res) => {
    res.send('Hello, Express!');
});
```
- `app.get(path, handler)`: `GET` 요청을 처리합니다.
- `app.post(path, handler)`: `POST` 요청을 처리합니다.

---

### 4. **경로 매개변수**

경로 매개변수는 URL 경로의 동적인 부분을 처리하는 방법입니다. 콜론(`:`)을 사용해 변수를 정의할 수 있습니다.

#### 예시:
```js
app.get('/r/:subreddit', (req, res) => {
    const { subreddit } = req.params;
    res.send(`<h1>This is ${subreddit} subreddit!<h1>`);
});
```
- **`req.params`**: 경로 매개변수 데이터를 담고 있는 객체입니다.
  - `/r/:subreddit`에서 `:subreddit`는 매개변수로, 클라이언트가 요청한 값을 받을 수 있습니다.

---

### 5. **중첩된 경로 매개변수**

여러 매개변수를 중첩하여 사용할 수도 있습니다.

#### 예시:
```js
app.get('/r/:subreddit/:postId', (req, res) => {
    const { subreddit, postId } = req.params;
    res.send(`<h1>Viewing Post ID: ${postId} on the ${subreddit} subreddit</h1>`);
});
```
- `/r/javascript/1234`로 요청하면:
  - `subreddit`: `javascript`
  - `postId`: `1234`를 반환합니다.

---

### 6. **쿼리 문자열**

쿼리 문자열은 URL에 데이터를 전달하는 방법입니다. `?` 뒤에 키-값 쌍으로 전달됩니다.

#### 예시:
```js
app.get('/search', (req, res) => {
    const { q } = req.query;
    if (!q) {
        res.send(`There is no result if nothing search!`);
    }
    res.send(`This is a result for ${q}!`);
});
```
- **`req.query`**: URL의 쿼리 문자열 데이터를 담고 있는 객체입니다.
  - `/search?q=express`로 요청하면 `q`의 값은 `express`가 됩니다.

---

### 7. **와일드카드 경로**

`*`를 사용하여 모든 경로를 매칭할 수 있습니다. 주로 존재하지 않는 경로에 대한 처리를 위해 사용됩니다.

#### 예시:
```js
app.get('*', (req, res) => {
    res.send(`I don't know that path!`);
});
```
- `/unknownpath`로 요청 시:
  - 응답: `"I don't know that path!"`

---

### 8. **서버 시작하기**

서버를 실행하려면 `app.listen()`을 사용합니다. 이는 애플리케이션이 특정 포트에서 클라이언트 요청을 수신하도록 설정합니다.

#### 예시:
```js
app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```
- 위 코드는 서버를 `http://localhost:3000`에서 실행합니다.

---

### 9. **요약된 예시**

아래는 위 개념들을 활용한 코드의 전체 구조입니다.

```js
const express = require('express');
const app = express();
const PORT = 3000;

app.get('/', (req, res) => {
    res.send('Hello, Express!');
});

app.get('/r/:subreddit', (req, res) => {
    const { subreddit } = req.params;
    res.send(`<h1>This is ${subreddit} subreddit!<h1>`);
});

app.get('/r/:subreddit/:postId', (req, res) => {
    const { subreddit, postId } = req.params;
    res.send(`<h1>Viewing Post ID: ${postId} on the ${subreddit} subreddit</h1>`);
});

app.post('/cats', (req, res) => {
    res.send('This is a post request, meow!');
});

app.get('/search', (req, res) => {
    const { q } = req.query;
    if (!q) {
        res.send(`There is no result if nothing search!`);
    }
    res.send(`This is a result for ${q}!`);
});

app.get('*', (req, res) => {
    res.send(`I don't know that path!`);
});

app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```
---

이 내용을 통해 Express의 기초를 익히고 간단한 애플리케이션을 작성해보세요!
