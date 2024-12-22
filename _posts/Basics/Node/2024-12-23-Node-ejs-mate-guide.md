---
title: "ejs-mate: EJS 템플릿을 강화하는 강력한 도구"
excerpt: "Node.js 환경에서 EJS 템플릿 엔진의 기능을 확장하고 코드 재사용성을 높이는 ejs-mate의 특징과 사용법을 알아봅니다."
categories:
  - Node
  
tags:
  - [Node.js, Express, EJS, Templating, Backend]
permalink: /Node/ejs-mate-guide/
toc: true
toc_sticky: true
date: 2024-12-22
last_modified_at: 2024-12-22
---

## ejs-mate란?

`ejs-mate`는 Node.js 환경에서 사용하는 `EJS`(Embedded JavaScript) 템플릿 엔진을 더 강력하게 만들어주는 미들웨어입니다. 주로 Express.js와 함께 사용되며, EJS 템플릿의 기능을 확장하고 코드 재사용성을 높이는 데 도움을 줍니다.

## 주요 기능 및 특징

1. **템플릿 레이아웃 지원**  
   `ejs-mate`는 템플릿의 레이아웃 기능을 지원합니다. 레이아웃은 페이지마다 반복되는 HTML 구조(예: 헤더, 푸터)를 한 곳에서 정의하고, 개별 페이지에서 해당 레이아웃을 확장하여 콘텐츠를 삽입할 수 있게 해줍니다.

2. **코드 재사용성**  
   템플릿 구성 요소를 분리하고 관리하기 쉽게 만들어주므로, 유지보수성과 재사용성을 높일 수 있습니다. 공통 요소(예: 네비게이션 바, 헤더, 푸터)를 별도의 파일로 분리해 관리할 수 있습니다.

3. **간편한 설정**  
   Express.js 앱에서 `ejs-mate`를 설정하면 추가적인 복잡한 설정 없이 템플릿 레이아웃 및 부분 렌더링 기능을 사용할 수 있습니다.

4. **EJS의 모든 기능 호환**  
   `ejs-mate`는 EJS의 기존 문법 및 기능과 완벽하게 호환되므로, 기존 EJS 프로젝트에 쉽게 통합할 수 있습니다.

---

## 설치 및 기본 사용법

### 1. 설치
```bash
npm install ejs ejs-mate
```

### 2. Express 앱 설정
```javascript
const express = require('express');
const path = require('path');
const ejsMate = require('ejs-mate');

const app = express();

// ejs-mate를 뷰 엔진으로 설정
app.engine('ejs', ejsMate);
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, 'views'));

// 라우트 예제
app.get('/', (req, res) => {
  res.render('home');
});

// 서버 실행
app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

---

### 3. 레이아웃 파일 작성
`views/layout.ejs`
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title><%= title || "My App" %></title>
</head>
<body>
  <header>
    <h1>공통 헤더</h1>
  </header>
  <main>
    <%- body %> <!-- 개별 페이지 내용 -->
  </main>
  <footer>
    <p>공통 푸터</p>
  </footer>
</body>
</html>
```

---

### 4. 페이지 작성
`views/home.ejs`
```html
<% layout('layout') %> <!-- 레이아웃 파일을 지정 -->
<h2>홈 페이지</h2>
<p>여기는 홈 페이지 내용입니다.</p>
```

---

## 장점
- **구조화된 템플릿 관리:** 레이아웃 기반으로 구조화된 템플릿 작성 가능.
- **빠른 학습 곡선:** EJS 문법과 Express 사용 경험이 있다면 쉽게 사용할 수 있음.
- **유지보수 용이:** 코드 재사용성이 높아져 유지보수가 쉬움.

## 단점
- **복잡한 프로젝트:** 규모가 매우 큰 프로젝트에서는 React, Vue, Angular 같은 프레임워크가 더 적합할 수 있음.
- **서버 사이드 렌더링:** 클라이언트 측 렌더링이 필요하다면 한계가 있을 수 있음.

---

`ejs-mate`는 EJS를 사용하는 Express 프로젝트에서 강력한 템플릿 관리 도구로, 코드 작성과 유지보수를 단순화해 줍니다. 위 가이드를 참고하여 프로젝트에 적용해보세요! 😊

