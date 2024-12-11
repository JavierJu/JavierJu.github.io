---
title: "Express와 EJS로 RESTful API 구현: Index, New, Create, Show, Edit, Update, Destroy"
excerpt: "Express와 EJS를 활용하여 간단한 블로그 게시물 관리 애플리케이션을 만들어보고, RESTful 라우팅을 GET, POST, PATCH, DELETE 메서드와 함께 구현하는 방법을 알아봅니다."
categories:
  - Node
tags:
  - [JavaScript, Express, EJS, RESTful, CRUD]
permalink: /Node/restful-api-example/
toc: true
toc_sticky: true
date: 2024-12-11
last_modified_at: 2024-12-11
---

### Express와 EJS를 사용한 RESTful API 구현

이 글에서는 **Express**와 **EJS**를 이용하여 RESTful 라우팅을 구현하는 방법을 설명합니다. 간단한 "블로그 게시물 관리" 애플리케이션을 모델로 삼아 각 라우트의 역할과 구현 방식을 코드와 함께 살펴보겠습니다.

---

### 프로젝트 설정

먼저 프로젝트를 초기화하고 필요한 패키지를 설치합니다.

```bash
# 프로젝트 폴더 생성 및 초기화
mkdir restful-blog
cd restful-blog
npm init -y

# Express와 EJS 설치
npm install express ejs body-parser method-override
```

**설명**:  
- `express`: 웹 애플리케이션 프레임워크  
- `ejs`: 템플릿 엔진  
- `body-parser`: 폼 데이터를 처리하기 위한 미들웨어  
- `method-override`: HTML 폼에서 PATCH와 DELETE 메서드를 지원하기 위한 라이브러리  

---

### 파일 구조

프로젝트는 다음과 같은 구조로 구성됩니다:

```plaintext
restful-blog/
├── public/
│   └── styles.css
├── views/
│   ├── index.ejs
│   ├── new.ejs
│   ├── show.ejs
│   ├── edit.ejs
├── app.js
├── package.json
└── README.md
```

---

### 코드 작성

#### 1. `app.js`: Express 애플리케이션 설정

```js
const express = require('express');
const bodyParser = require('body-parser');
const methodOverride = require('method-override');

const app = express();
const PORT = 3000;

// 미들웨어 설정
app.use(bodyParser.urlencoded({ extended: true }));
app.use(methodOverride('_method'));
app.set('view engine', 'ejs');
app.use(express.static('public'));

// 데이터 저장소 (임시)
let posts = [
  { id: 1, title: '첫 번째 글', content: '여기는 첫 번째 글 내용입니다.' },
  { id: 2, title: '두 번째 글', content: '여기는 두 번째 글 내용입니다.' },
];

// 라우트 정의

// Index: 모든 게시물 보기
app.get('/posts', (req, res) => {
  res.render('index', { posts });
});

// New: 새로운 게시물 작성 폼
app.get('/posts/new', (req, res) => {
  res.render('new');
});

// Create: 새로운 게시물 생성
app.post('/posts', (req, res) => {
  const newPost = {
    id: posts.length + 1,
    title: req.body.title,
    content: req.body.content,
  };
  posts.push(newPost);
  res.redirect('/posts');
});

// Show: 특정 게시물 보기
app.get('/posts/:id', (req, res) => {
  const post = posts.find(p => p.id === parseInt(req.params.id));
  if (post) {
    res.render('show', { post });
  } else {
    res.status(404).send('게시물을 찾을 수 없습니다.');
  }
});

// Edit: 게시물 수정 폼
app.get('/posts/:id/edit', (req, res) => {
  const post = posts.find(p => p.id === parseInt(req.params.id));
  if (post) {
    res.render('edit', { post });
  } else {
    res.status(404).send('게시물을 찾을 수 없습니다.');
  }
});

// Update: 게시물 수정
app.patch('/posts/:id', (req, res) => {
  const post = posts.find(p => p.id === parseInt(req.params.id));
  if (post) {
    post.title = req.body.title;
    post.content = req.body.content;
    res.redirect(`/posts/${post.id}`);
  } else {
    res.status(404).send('게시물을 찾을 수 없습니다.');
  }
});

// Destroy: 게시물 삭제
app.delete('/posts/:id', (req, res) => {
  posts = posts.filter(p => p.id !== parseInt(req.params.id));
  res.redirect('/posts');
});

// 서버 실행
const PORT = 3000;
app.listen(PORT, () => {
  console.log(`서버가 http://localhost:${PORT}에서 실행 중입니다.`);
});
```

---

### EJS 템플릿

#### 1. `index.ejs`: 게시물 목록

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>게시물 목록</title>
</head>
<body>
  <h1>게시물 목록</h1>
  <a href="/posts/new">새 게시물 작성</a>
  <ul>
    <% posts.forEach(post => { %>
      <li>
        <a href="/posts/<%= post.id %>"><%= post.title %></a>
        <form action="/posts/<%= post.id %>?_method=DELETE" method="POST" style="display:inline;">
          <button type="submit">삭제</button>
        </form>
        <a href="/posts/<%= post.id %>/edit">수정</a>
      </li>
    <% }); %>
  </ul>
</body>
</html>
```

#### 2. `new.ejs`: 게시물 작성 폼

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>새 게시물 작성</title>
</head>
<body>
  <h1>새 게시물 작성</h1>
  <form action="/posts" method="POST">
    <label for="title">제목:</label>
    <input type="text" id="title" name="title" required>
    <br>
    <label for="content">내용:</label>
    <textarea id="content" name="content" required></textarea>
    <br>
    <button type="submit">작성</button>
  </form>
  <a href="/posts">목록으로</a>
</body>
</html>
```

#### 3. `show.ejs`: 특정 게시물 보기

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title><%= post.title %></title>
</head>
<body>
  <h1><%= post.title %></h1>
  <p><%= post.content %></p>
  <a href="/posts">목록으로 돌아가기</a>
  <a href="/posts/<%= post.id %>/edit">수정</a>
</body>
</html>
```

#### 4. `edit.ejs`: 게시물 수정 폼

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>게시물 수정</title>
</head>
<body>
  <h1>게시물 수정</h1>
  <form action="/posts/<%= post.id %>?_method=PATCH" method="POST">
    <label for="title">제목:</label>
    <input type="text" id="title" name="title" value="<%= post.title %>" required>
    <br>
    <label for="content">내용:</label>
    <textarea id="content" name="content" required><%= post.content %></textarea>
    <br>
    <button type="submit">수정</button>
  </form>
  <a href="/posts">목록으로</a>
</body>
</html>
```

---

### 결론
이 코드는 간단한 RESTful CRUD 애플리케이션을 구현하는 방법을 보여줍니다. `EJS`는 데이터를 보여주기 위한 템플릿 엔진으로 사용되며, `method-override`를 사용하여 `PATCH`와 `DELETE` 요청을 HTML 폼에서 지원합니다. 이 구조를 통해 Express에서 RESTful 라우트를 체계적으로 구현할 수 있습니다.