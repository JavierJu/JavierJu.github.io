---
title: "JavaScript와 AJAX에서 사용되는 JSON 이해하기"
excerpt: "JSON의 구조, 특징, 그리고 AJAX와 함께 사용하는 방법에 대해 자세히 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, AJAX, JSON, Web Development, Data Exchange]
permalink: /JavaScript/json-with-ajax/
toc: true
toc_sticky: true
date: 2024-12-04
last_modified_at: 2024-12-04
---

## JSON이란?

JSON(JavaScript Object Notation)은 데이터를 교환하기 위해 사용되는 경량의 데이터 형식입니다. 사람이 읽고 쓰기 쉽고 기계가 해석하기도 간단하여 웹 개발에서 서버와 클라이언트 간의 데이터 전송에 널리 사용됩니다.

### JSON의 주요 특징

1. **가독성**: JSON은 사람이 읽고 이해하기 쉬운 텍스트 기반 포맷입니다.
2. **경량성**: 단순한 데이터 구조로 네트워크 대역폭을 효율적으로 사용합니다.
3. **언어 독립적**: 다양한 프로그래밍 언어에서 처리할 수 있는 표준 포맷입니다.

---

## JSON 형식

JSON 데이터는 두 가지 주요 데이터 구조를 포함합니다:

### 1. 객체 (Object)

`{}`로 감싸진 이름-값 쌍(`key: value`)으로 구성됩니다.

``` json
{
  "name": "John",
  "age": 30,
  "city": "New York"
}
```

### 2. 배열 (Array)

`[]`로 감싸진 값들의 리스트입니다.

``` json
{
  "names": ["Alice", "Bob", "Charlie"]
}
```

---

## JSON 데이터 예시

아래는 다양한 데이터 유형을 포함한 JSON 데이터 예시입니다:

``` json
{
  "name": "John",
  "age": 30,
  "isStudent": false,
  "address": {
    "street": "123 Main St",
    "city": "New York"
  },
  "courses": [
    {
      "title": "Math",
      "grade": "A"
    },
    {
      "title": "English",
      "grade": "B"
    }
  ]
}
```

---

## JSON과 JavaScript

### JSON을 JavaScript 객체로 변환

`JSON.parse()` 메서드를 사용합니다.

``` js
const jsonString = '{"name": "John", "age": 30}';
const jsonObj = JSON.parse(jsonString);
console.log(jsonObj.name); // "John"
```

### JavaScript 객체를 JSON 문자열로 변환

`JSON.stringify()` 메서드를 사용합니다.

``` js
const person = { name: "John", age: 30 };
const jsonString = JSON.stringify(person);
console.log(jsonString); // '{"name":"John","age":30}'
```

---

## AJAX에서 JSON 사용하기

AJAX는 서버와 비동기적으로 데이터를 교환할 수 있게 해줍니다. JSON은 이러한 데이터 교환에 매우 적합합니다.

### AJAX 요청으로 JSON 데이터 받기

``` js
const xhr = new XMLHttpRequest();
xhr.open("GET", "data.json", true);
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4 && xhr.status == 200) {
    const data = JSON.parse(xhr.responseText);
    console.log(data.name); // 서버에서 받은 데이터 사용
  }
};
xhr.send();
```

### 서버로 JSON 데이터 전송 (POST 요청)

``` js
const xhr = new XMLHttpRequest();
xhr.open("POST", "/submit-data", true);
xhr.setRequestHeader("Content-Type", "application/json");

const data = JSON.stringify({ name: "John", age: 30 });

xhr.onreadystatechange = function() {
  if (xhr.readyState == 4 && xhr.status == 200) {
    console.log("Data submitted successfully");
  }
};

xhr.send(data);
```

---

## 결론

JSON은 웹 개발에서 데이터를 교환하는 가장 보편적인 표준 형식입니다. JavaScript와 함께 사용될 때 직관적이며, AJAX와 결합하여 서버와의 비동기 통신을 간편하게 처리할 수 있습니다. JSON을 이해하고 활용하는 것은 현대 웹 개발의 필수적인 기술입니다.
