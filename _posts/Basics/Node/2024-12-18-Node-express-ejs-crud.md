---
title: "Express와 EJS를 활용한 RESTful CRUD: req/res 주요 메서드 정리"
excerpt: "Express와 EJS로 RESTful CRUD를 구축하면서 자주 사용하는 req와 res 객체의 주요 메서드와 속성을 정리합니다. 자세한 예제와 함께 배워보세요."
categories:
  - Node
tags:
  - [JavaScript, Express, Backend, RESTful, CRUD]
permalink: /Node/express-ejs-crud/
toc: true
toc_sticky: true
date: 2024-12-18
last_modified_at: 2024-12-18
---

Express와 EJS를 사용해 RESTful CRUD를 구축할 때 주로 사용하는 `req`와 `res` 객체의 메서드 및 속성에 대해 자세히 설명하겠습니다.

---

### **1. `req` 객체 (Request)**

`req` 객체는 클라이언트로부터 서버로 들어오는 요청과 관련된 정보를 담고 있습니다.

#### **(1) `req.body`**
- **설명**: POST, PUT 등의 요청에서 전달된 데이터(payload)를 담고 있습니다.
- **필요조건**: `body-parser` 또는 `express.json()` / `express.urlencoded()` 미들웨어를 설정해야 사용 가능합니다.
- **예제**:

  ```javascript
  app.use(express.json());
  app.use(express.urlencoded({ extended: true }));

  app.post('/create', (req, res) => {
      const { name, age } = req.body; // 클라이언트가 보낸 JSON 데이터
      res.send(`Name: ${name}, Age: ${age}`);
  });
  ```

#### **(2) `req.params`**
- **설명**: URL 경로에서 전달되는 동적 파라미터 값을 담고 있습니다.
- **예제**:

  ```javascript
  app.get('/user/:id', (req, res) => {
      const userId = req.params.id; // '/user/123' 요청 시 userId는 '123'
      res.send(`User ID: ${userId}`);
  });
  ```

#### **(3) `req.query`**
- **설명**: URL의 쿼리 문자열 (query string)에 포함된 값을 객체 형태로 반환합니다.
- **예제**:

  ```javascript
  app.get('/search', (req, res) => {
      const { keyword, page } = req.query; // '/search?keyword=nodejs&page=2'
      res.send(`Search: ${keyword}, Page: ${page}`);
  });
  ```

#### **(4) `req.method`**
- **설명**: HTTP 요청 메서드(GET, POST, PUT, DELETE 등)를 확인할 수 있습니다.
- **예제**:

  ```javascript
  app.all('/action', (req, res) => {
      res.send(`Method used: ${req.method}`);
  });
  ```

#### **(5) `req.url`**
- **설명**: 요청받은 URL 경로를 제공합니다.
- **예제**:

  ```javascript
  app.use((req, res) => {
      console.log(`Requested URL: ${req.url}`);
      res.send('Check console for URL');
  });
  ```

---

### **2. `res` 객체 (Response)**

`res` 객체는 서버가 클라이언트로 응답을 보낼 때 사용하는 객체입니다.

#### **(1) `res.render(view, locals)`**
- **설명**: EJS와 같은 템플릿 엔진을 사용하여 HTML을 렌더링한 결과를 클라이언트에 전송합니다.
- **매개변수**:
  - `view`: 렌더링할 EJS 파일의 이름.
  - `locals`: 템플릿 엔진에 전달할 데이터.
- **예제**:

  ```javascript
  app.get('/', (req, res) => {
      res.render('index', { title: 'Home Page', message: 'Welcome!' });
  });
  ```

#### **(2) `res.send(data)`**
- **설명**: 문자열, JSON, 버퍼 등을 클라이언트로 전송합니다.
- **특징**: `Content-Type` 헤더를 자동으로 설정합니다.
- **예제**:

  ```javascript
  app.get('/json', (req, res) => {
      res.send({ message: 'This is a JSON response' });
  });
  ```

#### **(3) `res.redirect(url)`**
- **설명**: 클라이언트를 다른 URL로 리다이렉트합니다.
- **예제**:

  ```javascript
  app.get('/old-page', (req, res) => {
      res.redirect('/new-page'); // '/old-page' 요청 시 '/new-page'로 이동
  });
  ```

#### **(4) `res.status(code)`**
- **설명**: HTTP 상태 코드를 설정합니다.
- **예제**:

  ```javascript
  app.get('/not-found', (req, res) => {
      res.status(404).send('Page not found');
  });
  ```

#### **(5) `res.json(data)`**
- **설명**: JSON 데이터를 클라이언트로 전송합니다. `res.send()`와 비슷하지만 명시적으로 JSON 포맷을 지정합니다.
- **예제**:

  ```javascript
  app.get('/api', (req, res) => {
      res.json({ success: true, data: [1, 2, 3] });
  });
  ```

#### **(6) `res.end()`**
- **설명**: 응답을 종료합니다. 데이터 없이 종료할 때 사용됩니다.
- **예제**:

  ```javascript
  app.get('/end', (req, res) => {
      res.write('Processing...');
      res.end(); // 응답 종료
  });
  ```

---

### **RESTful CRUD에서 활용 예제**

```javascript
const express = require('express');
const app = express();

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// READ: Get all items
app.get('/items', (req, res) => {
    res.send('Retrieve all items');
});

// READ: Get one item
app.get('/items/:id', (req, res) => {
    const { id } = req.params;
    res.send(`Retrieve item with ID: ${id}`);
});

// CREATE: Add a new item
app.post('/items', (req, res) => {
    const { name, price } = req.body;
    res.status(201).send(`Item created: Name - ${name}, Price - ${price}`);
});

// UPDATE: Update an item
app.put('/items/:id', (req, res) => {
    const { id } = req.params;
    const { name, price } = req.body;
    res.send(`Item ${id} updated: Name - ${name}, Price - ${price}`);
});

// DELETE: Remove an item
app.delete('/items/:id', (req, res) => {
    const { id } = req.params;
    res.send(`Item ${id} deleted`);
});

// Start server
app.listen(3000, () => console.log('Server running on http://localhost:3000'));
```

이 코드로 CRUD 요청을 수행하며 `req`와 `res` 메서드 및 속성을 다양하게 활용할 수 있습니다.

