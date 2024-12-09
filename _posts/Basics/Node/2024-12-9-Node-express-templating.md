---
title: "Node.js Express에서 템플릿 엔진과 Templating 이해하기"
excerpt: "Node.js Express에서 템플릿 엔진의 역할과 사용법, 그리고 Templating의 개념과 예제를 통해 동적 HTML 페이지 렌더링 과정을 자세히 살펴봅니다."
categories:
  - Node
tags:
  - [JavaScript, Node.js, Express, 템플릿 엔진, Backend]
permalink: /Node/express-templating/
toc: true
toc_sticky: true
date: 2024-12-9
last_modified_at: 2024-12-9
---

### 템플릿 엔진과 Templating이란?

Node.js에서 Express는 서버 사이드 애플리케이션을 개발하는 데 많이 사용되는 웹 프레임워크입니다. 이와 함께 **템플릿 엔진**을 사용하면 동적으로 데이터를 삽입한 HTML 페이지를 생성할 수 있습니다. 템플릿 엔진은 HTML 구조를 미리 정의하고 필요한 데이터를 삽입하거나 조건부 렌더링을 가능하게 해주는 도구입니다.

---

### 템플릿 엔진의 주요 기능
1. **데이터 삽입:** HTML 템플릿에 서버에서 제공하는 데이터를 동적으로 삽입.
2. **조건부 렌더링:** 특정 조건에 따라 HTML의 일부를 렌더링하거나 숨김.
3. **반복 처리:** 배열 데이터 기반으로 HTML 요소를 반복 렌더링.
4. **레이아웃 관리:** 공통적으로 사용되는 헤더, 푸터 등의 레이아웃을 재사용.

---

### Express에서 사용 가능한 템플릿 엔진

Express는 다양한 템플릿 엔진을 지원하며, 대표적으로 다음과 같은 엔진이 있습니다:

- **Pug:** 간결한 문법과 고급 기능을 제공.
- **EJS:** HTML과 유사한 문법으로 동적 렌더링 지원.
- **Handlebars:** 데이터와 템플릿을 명확히 분리하는 구조.
- **Mustache:** 간단한 문법으로 최소한의 기능 제공.

---

### Express에서 템플릿 엔진 사용하기

#### 1. Express 프로젝트 초기화
```bash
mkdir my-express-app
cd my-express-app
npm init -y
npm install express
```

#### 2. 템플릿 엔진 설치
사용할 템플릿 엔진을 설치합니다. 예를 들어, **Pug**를 사용하려면:
```bash
npm install pug
```

#### 3. 템플릿 엔진 설정
템플릿 엔진을 Express 애플리케이션에 등록합니다.
```js
const express = require('express');
const app = express();

// 템플릿 엔진 설정
app.set('view engine', 'pug');
app.set('views', './views'); // 템플릿 파일 경로 설정

// 기본 라우트 설정
app.get('/', (req, res) => {
    res.render('index', { title: 'Home', message: 'Welcome to Express with Pug!' });
});

// 서버 실행
app.listen(3000, () => {
    console.log('Server is running on http://localhost:3000');
});
```

#### 4. 템플릿 파일 작성
`views/index.pug` 파일을 생성합니다.
```pug
doctype html
html
  head
    title= title
  body
    h1= message
    p This is a Pug template example.
```

클라이언트에서 페이지를 요청하면, 위의 Pug 파일이 HTML로 렌더링되어 반환됩니다.

---

### EJS 예제

EJS는 HTML과 유사한 문법으로 템플릿을 작성할 수 있는 인기 있는 엔진입니다.

#### 설치
```bash
npm install ejs
```

#### Express 설정
```js
app.set('view engine', 'ejs');
app.set('views', './views');
```

#### EJS 템플릿 파일 작성 (`views/index.ejs`)
```html
<!DOCTYPE html>
<html>
<head>
    <title><%= title %></title>
</head>
<body>
    <h1><%= message %></h1>
    <p>This is an EJS template example.</p>
</body>
</html>
```

#### 라우트 설정
```js
app.get('/', (req, res) => {
    res.render('index', { title: 'Welcome', message: 'Hello, EJS!' });
});
```

---

### 템플릿 엔진의 활용
템플릿 엔진은 동적 HTML 생성뿐 아니라 조건부 렌더링, 배열 반복 등 다양한 작업에 활용됩니다.

#### 조건부 렌더링
- **Pug**
```pug
if condition
  p Condition is true
else
  p Condition is false
```

- **EJS**
```html
<% if (condition) { %>
    <p>Condition is true</p>
<% } else { %>
    <p>Condition is false</p>
<% } %>
```

#### 반복 렌더링
- **Pug**
```pug
ul
  each item in items
    li= item
```

- **EJS**
```html
<ul>
    <% items.forEach(function(item) { %>
        <li><%= item %></li>
    <% }); %>
</ul>
```

---

### 정적 파일 통합
Express에서 정적 파일(CSS, JS, 이미지 등)을 제공하려면 `express.static()`을 사용합니다.
```js
app.use(express.static('public'));
```

---

### 결론

템플릿 엔진은 Express 애플리케이션에서 동적 HTML을 생성하고 클라이언트로 전송하는 데 매우 유용합니다. 각 엔진은 고유의 문법과 특성을 가지고 있으니, 프로젝트 요구 사항에 따라 적합한 템플릿 엔진을 선택하세요.
