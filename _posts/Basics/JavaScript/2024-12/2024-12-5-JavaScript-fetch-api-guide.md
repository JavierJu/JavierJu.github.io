---  
title: "JavaScript Fetch API 완벽 가이드"  
excerpt: "Fetch API를 활용하여 HTTP 요청을 처리하는 방법과 주요 기능에 대해 알아봅니다. GET, POST 요청부터 응답 처리와 에러 핸들링까지 상세히 설명합니다."  
categories:  
  - JavaScript  
tags:  
  - [JavaScript, Fetch API, HTTP, Web Development, Async]  
permalink: /JavaScript/fetch-api-guide/  
toc: true  
toc_sticky: true  
date: 2024-12-5  
last_modified_at: 2024-12-5  
---  

# JavaScript Fetch API 완벽 가이드

`Fetch API`는 JavaScript에서 HTTP 요청을 보내고 응답을 처리하는 현대적인 방법을 제공합니다. 간단한 문법과 유연성 덕분에 비동기 작업을 쉽게 수행할 수 있습니다. 이 가이드는 Fetch API의 기본 사용법부터 고급 기능까지 단계별로 설명합니다.

---

## Fetch API 기본 개념

- **기본 형태**: `fetch(url, options)`  
  - `url`: 요청을 보낼 대상 URL  
  - `options`: 선택적으로 요청 방법, 헤더, 본문 등을 설정  

---

## Fetch API 기본 사용법

### GET 요청
```js  
fetch('https://jsonplaceholder.typicode.com/posts')  
  .then(response => response.json()) // JSON으로 변환  
  .then(data => console.log(data)) // 데이터를 처리  
  .catch(error => console.error('Error:', error)); // 에러 처리  
```  

### POST 요청
```js  
fetch('https://jsonplaceholder.typicode.com/posts', {  
  method: 'POST', // HTTP 메서드 지정  
  headers: {  
    'Content-Type': 'application/json', // 요청 헤더 추가  
  },  
  body: JSON.stringify({  
    title: 'New Post',  
    body: 'This is the content',  
    userId: 1,  
  }), // 요청 본문 추가  
})  
  .then(response => response.json())  
  .then(data => console.log(data))  
  .catch(error => console.error('Error:', error));  
```  

---

## Fetch API 응답 처리

### 응답 객체의 주요 메서드와 속성
- `response.ok`: 응답 상태 코드가 200~299 범위인지 확인 (Boolean)  
- `response.status`: HTTP 상태 코드 (예: 200, 404, 500)  
- `response.statusText`: 상태 코드에 대한 메시지 (예: "OK", "Not Found")  
- `response.json()`: JSON 형태의 응답 데이터를 JavaScript 객체로 변환 (Promise 반환)  
- `response.text()`: 텍스트 데이터로 응답을 반환 (Promise 반환)  
- `response.blob()`: 바이너리 데이터 (이미지, 파일 등)로 변환  
- `response.headers`: 응답 헤더 정보를 포함하는 `Headers` 객체  

#### 예시
```js  
fetch('https://jsonplaceholder.typicode.com/posts/1')  
  .then(response => {  
    if (!response.ok) {  
      throw new Error(`HTTP error! Status: ${response.status}`);  
    }  
    return response.json();  
  })  
  .then(data => console.log(data))  
  .catch(error => console.error('Fetch Error:', error));  
```  

---

## Fetch API 주요 옵션

### 옵션 객체
`fetch`의 두 번째 인수로 전달하는 옵션 객체를 통해 다양한 설정을 추가할 수 있습니다.

```js  
const options = {  
  method: 'POST', // 요청 방법: GET, POST, PUT, DELETE 등  
  headers: {  
    'Content-Type': 'application/json', // 요청 헤더  
    Authorization: 'Bearer YOUR_TOKEN', // 인증 헤더  
  },  
  body: JSON.stringify({ key: 'value' }), // 요청 본문 (POST/PUT 등에서 사용)  
  mode: 'cors', // 요청 모드: cors, no-cors, same-origin  
  cache: 'no-cache', // 캐싱 정책  
  credentials: 'include', // 쿠키 포함 여부: omit, same-origin, include  
};  
```  

#### 예시
```js  
fetch('https://api.example.com/data', options)  
  .then(response => response.json())  
  .then(data => console.log(data))  
  .catch(error => console.error('Error:', error));  
```  

---

## 비동기/동기 처리

### Async/Await 사용
```js  
async function fetchData() {  
  try {  
    const response = await fetch('https://jsonplaceholder.typicode.com/posts');  
    if (!response.ok) {  
      throw new Error(`HTTP error! Status: ${response.status}`);  
    }  
    const data = await response.json();  
    console.log(data);  
  } catch (error) {  
    console.error('Error:', error);  
  }  
}  

fetchData();  
```  

---

## AbortController를 활용한 요청 취소
```js  
const controller = new AbortController();  
const signal = controller.signal;  

setTimeout(() => controller.abort(), 5000); // 5초 후 요청 취소  

fetch('https://jsonplaceholder.typicode.com/posts', { signal })  
  .then(response => response.json())  
  .then(data => console.log(data))  
  .catch(error => {  
    if (error.name === 'AbortError') {  
      console.error('Fetch aborted!');  
    } else {  
      console.error('Error:', error);  
    }  
  });  
```  

---

## 결론

`Fetch API`는 간단한 문법과 유연성을 제공하여 HTTP 요청을 처리하는 데 적합합니다. 기존의 `XMLHttpRequest`를 대체하며 다양한 옵션과 결합하여 강력하게 활용할 수 있습니다.
