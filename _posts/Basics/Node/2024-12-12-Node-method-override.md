---
title: "Express의 method-override: HTML 폼에서 PUT 및 DELETE 지원"
excerpt: "Express.js에서 method-override를 사용하여 HTML 폼에서 PUT, DELETE와 같은 HTTP 메서드를 지원하는 방법을 알아봅니다. 설치 방법, 주요 사용 사례, 보안 고려 사항까지 설명합니다."
categories:
  - Node
tags:
  - [JavaScript, Express, REST API, Backend]
permalink: /Node/method-override/
toc: true
toc_sticky: true
date: 2024-12-12
last_modified_at: 2024-12-12
---

`method-override`는 [`Express.js`](https://expressjs.com/) 애플리케이션에서 HTML 폼이 지원하지 않는 HTTP 메서드(예: PUT, DELETE 등)를 사용할 수 있도록 해주는 미들웨어입니다. 이 글에서는 `method-override`의 설치, 사용법, 주요 사례, 보안 고려 사항 등을 다룹니다.

## 1. 주요 사용 사례
1.  HTML 폼에서 PUT 및 DELETE 요청 사용
RESTful 라우팅에서 자주 사용되는 PUT과 DELETE는 폼의 기본 메서드(GET/POST)로는 사용할 수 없습니다.
이때 `method-override`를 사용하면 POST 요청의 쿼리나 헤더를 기반으로 HTTP 메서드를 오버라이드하여 사용할 수 있습니다.

2. RESTful API 설계 지원
REST API를 설계할 때 클라이언트가 모든 HTTP 메서드를 사용할 수 없는 환경이라면, `method-override`로 메서드를 대체할 수 있습니다.

## 2. 설치 방법
`method-override`는 npm 패키지입니다. 다음 명령어로 설치할 수 있습니다:

```bash
npm install method-override
```

## 3. 기본 사용법
### 코드 예제

```js
const express = require('express');
const methodOverride = require('method-override');

const app = express();

// 1. Query parameter 기반 method override
app.use(methodOverride('_method'));

// 2. JSON 데이터 처리
app.use(express.json());

// 3. 폼 데이터 처리
app.use(express.urlencoded({ extended: true }));

// 예제 라우트
app.get('/', (req, res) => {
res.send( <form action="/resource?_method=DELETE" method="POST"> <button type="submit">DELETE</button> </form> );
});

app.delete('/resource', (req, res) => {
res.send('Resource deleted');
});

app.listen(3000, () => {
console.log('Server running on http://localhost:3000');
});
```

### 동작 설명

1. 클라이언트가 `_method`라는 쿼리 매개변수를 추가한 POST 요청을 보냅니다.

2. `method-override`가 `_method` 값을 확인하고 요청 메서드를 해당 값으로 대체합니다.

  - 예: `_method=DELETE`라면, 요청은 DELETE 메서드로 처리됩니다.

3. Express 라우터는 대체된 메서드에 따라 요청을 처리합니다.

## 4. 오버라이드 전략

### 1) Query String (쿼리 매개변수)

```js
app.use(methodOverride('_method'));
```

요청 예:

```html
<form action="/resource?_method=PUT" method="POST"> 
<input type="text" name="name" value="New Name" /> 
<button type="submit">Update</button> 
</form>
```

### 2) HTTP Header
클라이언트가 쿼리 매개변수를 사용할 수 없을 경우, 요청 헤더를 통해 메서드를 지정할 수도 있습니다.

```js
app.use(methodOverride((req, res) => {
if (req.headers['x-http-method-override']) {
return req.headers['x-http-method-override'];
}
}));
```

요청 예:

```http
POST /resource HTTP/1.1
Host: localhost:3000
X-HTTP-Method-Override: DELETE
Content-Type: application/json

{ "id": 123 }
```

### 3) Body Parameter
폼 데이터의 숨겨진 필드로 메서드를 지정할 수도 있습니다.

```js
app.use(methodOverride('_method'));
```

HTML 폼 예:

```html
<form action="/resource" method="POST"> 
<input type="hidden" name="_method" value="DELETE"> 
<button type="submit">DELETE</button> 
</form> 
```

## 5. 보안 고려 사항
1. 의도치 않은 메서드 오버라이드 방지
클라이언트가 요청 헤더 또는 쿼리를 통해 메서드를 악의적으로 변경할 가능성이 있습니다. 특정 조건에서만 메서드를 오버라이드하도록 제한하세요.

```js
app.use(methodOverride((req, res) => {
if (req.body && typeof req.body === 'object' && '_method' in req.body) {
return req.body._method;
}
}));
```

2. CORS 및 인증 확인
메서드 오버라이드 요청이 의도한 클라이언트에서만 오도록 인증 및 CORS 설정을 확인하세요.

## 6. 정리
- `method-override`는 Express 애플리케이션에서 HTTP 메서드의 제한을 해결하는 간단한 도구입니다.
- 주로 RESTful 설계와 HTML 폼의 제약을 극복하기 위해 사용됩니다.
- 보안 고려 사항을 염두에 두고, 상황에 적합한 전략을 선택하여 사용하세요.