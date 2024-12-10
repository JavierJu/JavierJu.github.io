---
title: "Express에서 POST 요청 처리: bodyParser와 req.body의 이해"
excerpt: "Express에서 POST 요청 본문 데이터를 처리하기 위한 bodyParser의 역할과 사용 방법을 알아봅니다. x-www-form-urlencoded 데이터와 extended 옵션의 개념도 함께 설명합니다."
categories:
  - Node
tags:
  - [JavaScript, Express, Backend, POST 요청]
permalink: /Node/post-request-bodyparser/
toc: true
toc_sticky: true
date: 2024-12-10
last_modified_at: 2024-12-10
---

Express에서 POST 요청 본문을 처리할 때 `req.body`에 접근하기 위해 `app.use(bodyParser.urlencoded({ extended: true }))`를 사용하는데, 이 미들웨어와 관련된 개념을 자세히 알아봅시다.

## body-parser 미들웨어란?
`body-parser`는 Express에서 요청 본문을 파싱하여 `req.body` 객체로 만들어주는 미들웨어입니다. 요청 본문은 기본적으로 문자열 또는 버퍼 형식으로 전달되며, 이를 JavaScript 객체로 변환해야 쉽게 사용할 수 있습니다.

> Express 4.16.0 이상에서는 `body-parser`가 Express에 내장되어 있으며, `express.urlencoded` 및 `express.json`으로 대체 가능합니다.

---

## x-www-form-urlencoded란?
`x-www-form-urlencoded`는 HTML 폼 데이터가 서버로 전송될 때 사용하는 기본 인코딩 방식으로, 데이터가 다음과 같은 형식으로 인코딩됩니다:

```
key1=value1&key2=value2
```

이를 `body-parser`를 통해 JavaScript 객체로 변환하면 다음과 같이 사용할 수 있습니다:

```js
key1=value1&key2=value2 → { key1: "value1", key2: "value2" }
```


---

## extended 옵션의 역할
`app.use(bodyParser.urlencoded({ extended: true }))`에서 사용하는 `extended` 옵션은 데이터를 파싱하는 방식을 결정합니다.

- **`extended: true`**  
  - `qs` 라이브러리를 사용해 파싱합니다.
  - 중첩된 객체와 배열을 처리할 수 있습니다.
    ```js
    // Example
    foo[bar]=baz → { foo: { bar: 'baz' } }
    ```
  
- **`extended: false`**  
  - `querystring` 라이브러리를 사용해 파싱합니다.
  - 단순한 키-값 쌍만 처리 가능합니다.
    ```js
    foo=bar → { foo: 'bar' }
    ```

---

## 사용 예제
아래는 Express에서 `bodyParser`를 활용하여 POST 요청 데이터를 처리하는 예제입니다.

```js
const express = require('express');
const app = express();

// x-www-form-urlencoded 데이터 처리
app.use(express.urlencoded({ extended: true }));

app.post('/submit', (req, res) => {
  console.log(req.body); // 폼 데이터 출력
  res.send('Data received!');
});

app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

### 요청 예시
클라이언트에서 다음과 같은 폼 데이터를 전송했다면:

```
name=John&age=30
```


`req.body`는 다음 객체로 파싱됩니다:
```js
{ name: 'John', age: '30' }
```

---

## 왜 사용하는가?
1. **요청 본문 파싱**  
   본문 데이터를 JavaScript 객체로 변환하여 코드에서 쉽게 다룰 수 있습니다.
2. **코드 간소화**  
   직접 요청 데이터를 파싱하는 복잡함을 줄여줍니다.
3. **유연한 데이터 처리**  
   `extended: true`를 사용하면 중첩된 데이터 구조도 처리 가능합니다.

---

## 정리
- `bodyParser.urlencoded({ extended: true })`는 **x-www-form-urlencoded** 데이터를 JavaScript 객체로 변환하는 데 사용됩니다.
- `extended` 옵션은 중첩된 데이터 구조를 처리할 수 있는지 여부를 결정합니다.
- 최신 Express 버전에서는 별도의 `body-parser` 설치 없이도 `express.urlencoded`를 사용할 수 있습니다.

이해한 내용을 바탕으로 POST 요청 데이터를 처리하는 Express 애플리케이션을 효율적으로 개발해보세요!


