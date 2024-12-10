---
title: "파싱(Parsing)이란? 개념과 Express에서의 활용"
excerpt: "데이터를 구조화된 형식으로 변환하는 파싱의 개념과 Express에서의 POST 요청 본문 파싱 방법을 예제와 함께 설명합니다."
categories:
  - Node
tags:
  - [JavaScript, Express, Parsing, Backend]
permalink: /Node/parsing-concept-and-express/
toc: true
toc_sticky: true
date: 2024-12-10
last_modified_at: 2024-12-10
---

파싱(Parsing)은 데이터를 분석하고 처리 가능한 구조로 변환하는 과정을 의미합니다. 본 글에서는 파싱의 기본 개념과 Express에서 POST 요청 본문을 처리하기 위한 파싱 작업을 설명합니다.

---

## 파싱이란?
파싱은 데이터를 **구조화된 형식으로 변환**하는 과정입니다. 일반적으로 원시(raw) 데이터를 컴퓨터가 이해할 수 있는 구조로 바꿔서 활용할 수 있게 만듭니다.

### 예시: x-www-form-urlencoded 데이터
클라이언트가 다음과 같은 데이터를 서버로 보냈다고 가정합니다:

```
name=John&age=30
```

이 데이터를 바로 사용하기 어렵기 때문에, 파싱 과정을 통해 다음과 같이 구조화된 JavaScript 객체로 변환합니다:

```js
{
  name: "John",
  age: "30"
}
```

---

## 왜 파싱이 필요한가?
1. **데이터 구조화**  
   데이터를 분석하여 정리된 형태로 변환해야 코드에서 쉽게 사용할 수 있습니다.
2. **편리한 접근**  
   구조화된 데이터는 객체 형태로 접근이 가능하므로 처리 속도가 빨라지고 효율적입니다.

---

## 파싱의 동작 방식
데이터를 파싱하는 과정은 다음과 같이 진행됩니다:

### 1. **구조 분석**
원시 데이터를 규칙에 따라 나누어 분석합니다.  
예: `name=John&age=30`를 `&` 기준으로 나눔.

```js
['name=John', 'age=30']
```

### 2. **더 세부적으로 분석**
각 부분을 다시 `=` 기준으로 나누어 키와 값을 구분합니다.

```js
{ name: "John", age: "30" }
```

### 3. **데이터 변환**
분석된 결과를 JavaScript 객체로 변환하여 애플리케이션에서 사용할 수 있게 만듭니다.

---

## Express에서의 파싱
Express는 클라이언트 요청 본문을 처리하기 위해 `body-parser` 또는 내장된 미들웨어인 `express.urlencoded`와 `express.json`을 제공합니다.

### 예제: x-www-form-urlencoded 데이터 파싱

```js
const express = require('express');
const app = express();

// POST 요청 본문 파싱
app.use(express.urlencoded({ extended: true }));

app.post('/submit', (req, res) => {
  console.log(req.body); // 파싱된 데이터 출력
  res.send('Data received!');
});

app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

#### 요청 예시
클라이언트가 다음 데이터를 전송:

```
name=John&age=30
```


파싱 후 `req.body`의 내용:

```js
{ name: "John", age: "30" }
```

---

## Express의 `extended` 옵션
`express.urlencoded({ extended: true })`에서 `extended` 옵션은 데이터 파싱 방식을 결정합니다.

1. **`extended: true`**  
   - `qs` 라이브러리를 사용.
   - 중첩된 객체와 배열을 처리 가능.

     ```js
     // Example
     foo[bar]=baz → { foo: { bar: 'baz' } }
     ```

2. **`extended: false`**  
   - `querystring` 라이브러리를 사용.
   - 단순한 키-값 쌍만 처리 가능.
   
     ```js
     foo=bar → { foo: 'bar' }
     ```

---

## 다양한 파싱 사례
### 1. **JSON 파싱**
JSON 데이터를 문자열로 받을 경우:

```js
const jsonString = '{"name": "John", "age": 30}';
const parsedData = JSON.parse(jsonString);
console.log(parsedData); // { name: "John", age: 30 }
```

### 2. **HTML 폼 데이터 파싱**
HTML 폼에서 전송된 데이터:

```html
<form method="POST" action="/submit">
  <input type="text" name="name" value="John">
  <input type="text" name="age" value="30">
  <button type="submit">Submit</button>
</form>
```

전송된 데이터를 파싱하면: 

```js 
{ name: "John", age: "30" }
```

## 결론
파싱은 데이터를 분석하고 처리 가능한 구조로 변환하는 중요한 과정입니다. Express에서는 요청 본문 데이터를 쉽게 처리할 수 있도록 `body-parser` 또는 내장 미들웨어를 제공합니다. 이를 통해 클라이언트 데이터를 JavaScript 객체로 변환하고, 애플리케이션에서 효율적으로 사용할 수 있습니다.

파싱 과정을 이해하면 더 복잡한 데이터 구조를 다루는 데 큰 도움이 됩니다. Express를 활용한 데이터 처리에 활용해 보세요!
