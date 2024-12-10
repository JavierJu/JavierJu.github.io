---
title: "Express와 EJS 템플릿을 활용한 동적 HTML 생성 실습"
excerpt: "Express.js와 EJS 템플릿 엔진을 사용해 동적 웹 페이지를 생성하는 방법을 실습합니다. EJS 조건문, 루프, 정적 파일 사용, 템플릿 파일 분할, Bootstrap 적용 등 다양한 기능을 구현해 보세요."
categories:
  - Node
tags:
  - [JavaScript, Node.js, Express, EJS, Backend]
permalink: /Node/ejs-template-example/
toc: true
toc_sticky: true
date: 2024-12-10
last_modified_at: 2024-12-10
---

## Express와 EJS 템플릿으로 동적 HTML 구성하기

이 글에서는 Express.js와 EJS 템플릿 엔진을 사용하여 동적 HTML 페이지를 생성하는 방법을 단계별로 설명합니다. 아래의 주요 내용을 다룹니다:

1. 뷰 디렉토리 설정
2. 템플릿에 데이터 전달
3. EJS 조건문과 루프 활용
4. 정적 파일 사용
5. Bootstrap 적용
6. EJS 파일 분할

### Express 애플리케이션 설정

먼저 Express 애플리케이션을 설정하고, EJS를 템플릿 엔진으로 지정합니다. `views` 디렉토리와 정적 파일 경로도 설정합니다.

```js
const express = require('express'); 
const app = express();
const PORT = 3000;
const path = require('path');
const redditData = require('./data.json');

app.use(express.static(path.join(__dirname, '/public')));

app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, '/views'));

app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```

### 라우팅 및 EJS 템플릿 활용

#### 홈 페이지 라우트

`/` 경로에 대한 기본 라우트를 설정하여 `home.ejs` 템플릿을 렌더링합니다.

```js
app.get('/', (req, res) => {
    res.render('home');
});
```

#### 고양이 리스트 페이지

`/cats` 경로는 EJS 루프를 활용하여 고양이 이름 배열을 화면에 출력합니다.

```js
app.get('/cats', (req, res) => {
    const cats = [
        "Luna", "Oliver", "Milo", "Leo", "Charlie", 
        "Simba", "Max", "Bella", "Loki", "Nala"
    ];
    res.render('cats', { cats });
});
```

#### 랜덤 숫자 생성 페이지

`/rand` 경로에서 무작위 숫자를 생성해 EJS 템플릿에 전달합니다.

```js
app.get('/rand', (req, res) => {
    const num = Math.floor(Math.random() * 10) + 1;
    res.render('random', { rand: num, name: 'random' });
});
```

#### 서브레딧 페이지

`/r/:subreddit` 경로에서 서브레딧 데이터를 JSON 파일에서 가져와 화면에 출력합니다. 데이터가 없는 경우 `notfound.ejs` 템플릿을 렌더링합니다.

```js
app.get('/r/:subreddit', (req, res) => {
    const { subreddit } = req.params;
    const data = redditData[subreddit];
    if (data) {
        res.render('subreddit', { ...data });
    } else {
        res.render('notfound', { subreddit });
    }
});
```

### EJS 템플릿 구조

#### `subreddit.ejs`

서브레딧 정보를 표시하는 템플릿입니다. EJS 조건문과 루프를 활용하여 데이터를 출력합니다.

```html
<%- include('partials/head') %>

<%- include('partials/navbar') %>

<div class="container">
    <h1>Browsing the <%= name %> subreddit</h1>
    <h2><%= description %></h2>
    <p><%= subscribers %> total subscribers</p>
    <hr>
    <% for (let post of posts) { %>
        <article>
            <p><%= post.title %> - <b><%= post.author %></b></p>
            <% if (post.img) { %>
                <img src="<%= post.img %>" alt="">
            <% } %>
        </article>
    <% } %>
</div>

<%- include('partials/footer') %>
```

#### 공통 템플릿 파일

##### `partials/head.ejs`

HTML 문서의 기본 메타데이터와 정적 파일 경로를 설정합니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Site</title>
    <link rel="stylesheet" href="/css/bootstrap.min.css">
    <script src="/js/jquery-3.7.1.min.js"></script>
    <script src="/js/bootstrap.min.js"></script>
</head>
<body>
```

##### `partials/navbar.ejs`

Bootstrap 네비게이션 바를 생성합니다.

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container-fluid">
        <a class="navbar-brand" href="#">Express Demo</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"
            aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
                <li class="nav-item"><a class="nav-link" href="/">Home</a></li>
                <li class="nav-item"><a class="nav-link" href="/rand">Random Numbers</a></li>
                <li class="nav-item"><a class="nav-link" href="/cats">Cats</a></li>
                <li class="nav-item"><a class="nav-link" href="/r/soccer">Soccer</a></li>
                <li class="nav-item"><a class="nav-link" href="/r/chickens">Chickens</a></li>
                <li class="nav-item"><a class="nav-link" href="/r/mightyharvest">Mighty Harvest</a></li>
            </ul>
        </div>
    </div>
</nav>
```

##### `partials/footer.ejs`

푸터를 정의합니다.

```html
<hr>
<footer>
    <p>This is footer!</p>
</footer>
</body>
</html>
```

### 정적 Assets

`public` 디렉토리 아래에 정적 파일(예: CSS, JS)을 저장하고 `/css/bootstrap.min.css` 및 `/js/bootstrap.min.js` 등을 링크합니다.

### 정리

이 실습을 통해 Express.js와 EJS를 활용한 동적 웹 페이지 생성의 기초를 익혔습니다. EJS의 강력한 조건문, 루프 기능과 템플릿 분할의 유용성을 경험해 보세요!
