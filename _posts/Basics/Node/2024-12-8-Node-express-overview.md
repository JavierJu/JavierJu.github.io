---
title: "Express.js: Node.js 웹 개발의 강력한 도구"
excerpt: "Express.js의 주요 특징과 구성 요소, 사용 예제를 통해 웹 서버와 RESTful API를 구축하는 방법을 배우고, Express.js의 장단점과 활용 사례를 알아봅니다."
categories:
  - Node
tags:
  - [JavaScript, Node.js, Express, Backend, 웹 개발]
permalink: /Node/express-overview/
toc: true
toc_sticky: true
date: 2024-12-8
last_modified_at: 2024-12-8
---

### Express.js란 무엇인가요?

Express.js(이하 Express)는 **Node.js의 가장 널리 사용되는 웹 애플리케이션 프레임워크**로, **웹 서버 및 API를 빠르고 간단하게 구축**할 수 있도록 설계되었습니다. Express는 미니멀리즘과 유연성을 중시하며, 개발자가 필요에 따라 원하는 기능을 추가하거나 설정할 수 있는 방식으로 설계되었습니다.

Express는 Node.js의 기본 HTTP 모듈 위에 추상화를 추가하여, HTTP 요청 및 응답 처리를 더 직관적이고 효율적으로 만들었습니다.

---

### Express.js의 주요 특징

1. **간결한 코드 구조**  
   - HTTP 요청과 응답을 처리하는 데 필요한 로직을 단순화합니다.  
   - `req`, `res` 객체를 통해 요청 데이터를 쉽게 처리할 수 있습니다.

2. **미들웨어 기반 아키텍처**  
   - 요청과 응답 처리 흐름을 체인처럼 구성할 수 있습니다.  
   - 미들웨어는 인증, 로깅, 파싱, 에러 처리 등을 추가하는 데 사용됩니다.

3. **라우팅 시스템**  
   - URL 경로와 HTTP 메서드(GET, POST, PUT, DELETE 등)를 조합하여 다양한 엔드포인트를 정의할 수 있습니다.

4. **플러그인 및 확장성**  
   - 다양한 플러그인(예: 인증, 데이터베이스 연결)을 쉽게 통합할 수 있습니다.  
   - 커뮤니티에서 제공하는 수많은 모듈과 통합 가능합니다.

5. **템플릿 엔진 지원**  
   - HTML 템플릿 엔진(예: Pug, EJS, Handlebars)을 통해 동적 HTML 페이지를 생성할 수 있습니다.

6. **비동기 처리**  
   - Node.js의 비동기 특성을 그대로 활용하며, 비동기 요청 처리를 효율적으로 수행합니다.

---

### Express.js의 주요 구성 요소

1. **앱 객체 (`app`)**  
   Express 애플리케이션의 핵심으로, 모든 설정, 미들웨어, 라우팅 등을 관리합니다.  

   ```js
   const express = require('express');
   const app = express();
   ```

2. **라우터 (`Router`)**  
   HTTP 요청 경로를 정의하며, 요청 메서드와 URL을 기준으로 요청을 처리합니다.  

   ```js
   app.get('/', (req, res) => {
       res.send('Hello, World!');
   });
   ```

3. **미들웨어**  
   요청-응답 주기에 기능을 추가하거나 가로챌 수 있는 함수입니다.  

   ```js
   app.use((req, res, next) => {
       console.log(`${req.method} ${req.url}`);
       next(); // 다음 미들웨어로 이동
   });
   ```

4. **요청 객체 (`req`)**  
   클라이언트의 요청 데이터를 포함합니다. (예: URL, 헤더, 바디 등)

5. **응답 객체 (`res`)**  
   서버가 클라이언트에 응답을 보낼 때 사용하는 메서드와 속성을 포함합니다.

---

### Express.js의 주요 기능 및 사용 예제

#### 1. **기본 서버 설정**
Express로 서버를 설정하고 실행할 수 있습니다.

```js
const express = require('express');
const app = express();

const PORT = 3000;

// 기본 라우트
app.get('/', (req, res) => {
    res.send('Hello, Express!');
});

// 서버 실행
app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```

#### 2. **라우팅**
다양한 URL 경로와 HTTP 메서드에 따라 요청을 처리합니다.

```js
app.get('/users', (req, res) => {
    res.json([{ id: 1, name: 'John' }]);
});

app.post('/users', (req, res) => {
    res.status(201).send('User created');
});
```

#### 3. **미들웨어 추가**
요청 처리 과정을 확장하거나 조정할 수 있습니다.

```js
const bodyParser = require('body-parser');

app.use(bodyParser.json()); // JSON 요청 본문을 파싱
app.use((req, res, next) => {
    console.log(`${new Date().toISOString()} - ${req.method} ${req.url}`);
    next();
});
```

#### 4. **정적 파일 서비스**
HTML, CSS, JavaScript 파일 같은 정적 파일을 제공할 수 있습니다.

```js
app.use(express.static('public'));
```

#### 5. **템플릿 엔진 사용**
템플릿 엔진을 통해 동적 HTML 페이지를 렌더링합니다.

```js
app.set('view engine', 'pug');

app.get('/hello', (req, res) => {
    res.render('index', { title: 'Express', message: 'Hello, Pug!' });
});
```

#### 6. **에러 처리**
커스텀 에러 핸들러를 설정하여 에러를 관리할 수 있습니다.

```js
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).send('Something broke!');
});
```

---

### Express.js의 장단점

#### 장점
1. **빠르고 가볍다**: 불필요한 기능을 배제한 미니멀리즘 디자인.
2. **유연성**: 다양한 플러그인을 통해 확장이 용이.
3. **커뮤니티 지원**: 풍부한 오픈 소스 모듈과 강력한 사용자 커뮤니티.
4. **미들웨어 시스템**: 요청 흐름 제어가 간단하고 강력.

#### 단점
1. **유연성의 양날의 검**: 표준이 없어서 프로젝트 구조가 개발자마다 다를 수 있음.
2. **비교적 단순한 기능 제공**: 고급 기능은 직접 구현하거나 별도의 라이브러리를 사용해야 함.

---

### Express.js를 시작하기 위한 기본 설치

1. **Node.js 설치**  
   Express는 Node.js 환경에서 실행되므로, Node.js를 먼저 설치해야 합니다.

2. **Express 설치**  
   다음 명령어로 Express를 설치합니다.

   ```bash
   npm install express
   ```

3. **프로젝트 초기화**

   `package.json` 파일을 생성하고 Express를 설치하여 사용할 수 있습니다. 아래 명령어를 사용합니다.

   ```bash
   npm init -y
   npm install express
   ```

---

### Express.js의 활용 예시

#### RESTful API 서버 구축

Express는 RESTful API 서버를 구축하는 데 적합한 프레임워크입니다. 아래는 간단한 To-Do List API 예제입니다.

```js
const express = require('express');
const app = express();

// JSON 요청 본문 파싱 미들웨어 추가
app.use(express.json());

// To-Do 항목을 저장할 배열
let todos = [];

// 모든 To-Do 항목 가져오기
app.get('/todos', (req, res) => {
    res.json(todos);
});

// 새로운 To-Do 항목 추가
app.post('/todos', (req, res) => {
    const todo = { id: Date.now(), ...req.body };
    todos.push(todo);
    res.status(201).json(todo);
});

// 특정 To-Do 항목 삭제
app.delete('/todos/:id', (req, res) => {
    todos = todos.filter(todo => todo.id !== parseInt(req.params.id));
    res.status(204).send();
});

// 서버 실행
app.listen(3000, () => console.log('Server running on port 3000'));
```
---

### 마무리

Express.js는 웹 애플리케이션과 API를 구축하기 위한 강력하고 간결한 도구입니다. 미들웨어와 라우팅 기능을 활용하면 다양한 기능을 가진 서버를 효율적으로 개발할 수 있습니다. 단순한 시작점으로, 커스터마이징을 통해 복잡한 애플리케이션까지 확장 가능하다는 점에서 Express는 초보자와 숙련된 개발자 모두에게 적합합니다.
