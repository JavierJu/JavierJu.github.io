---  
title: "JavaScript 비동기 함수(async function)의 개념과 활용법"  
excerpt: "JavaScript의 비동기 함수(async function)에 대해 알아보고, 이를 활용해 비동기 작업을 효율적으로 처리하는 방법을 소개합니다."  
categories:  
  - JavaScript  
tags:  
  - [JavaScript, Async, Await, Promise, 비동기처리]  
permalink: /JavaScript/async-function/  
toc: true  
toc_sticky: true  
date: 2024-12-3
last_modified_at: 2024-12-3  
---  

### JavaScript 비동기 함수란?  
비동기 함수(`async function`)는 **JavaScript에서 비동기 작업을 쉽게 처리하기 위해 사용하는 함수**입니다.  
특히 시간이 오래 걸리는 작업(예: API 호출, 파일 읽기 등)을 처리할 때 코드의 흐름을 효율적으로 관리할 수 있습니다.  

#### **기본 문법**  
`async` 키워드를 함수 선언 앞에 붙이면 비동기 함수가 됩니다. 비동기 함수는 항상 `Promise` 객체를 반환합니다.  
```js  
async function myAsyncFunction() {  
  return "Hello, world!";  
}  

myAsyncFunction().then(result => console.log(result));  // "Hello, world!"  
```  

위의 예에서 함수는 `Promise.resolve("Hello, world!")`와 같은 결과를 반환합니다.  

---

### Await 키워드: 비동기를 동기처럼  
`await` 키워드는 `Promise`를 **기다리는 역할**을 합니다.  
비동기 함수 내에서 `await`를 사용하면 해당 `Promise`가 처리될 때까지 기다린 뒤, 그 결과 값을 반환받습니다.  

#### **사용 예시**  
```js  
async function fetchData() {  
  const response = await fetch("https://api.example.com/data"); // 비동기 처리 대기  
  const data = await response.json(); // 응답을 JSON으로 변환  
  console.log(data);  
}  

fetchData();  
```  

이 코드는 비동기 API 호출의 결과를 마치 동기 코드처럼 작성할 수 있게 합니다.  

---

### Async/Await와 Promise의 비교  
비동기 코드는 **Promise chaining** 방식으로도 처리할 수 있지만, `async/await`는 코드를 더 간결하고 읽기 쉽게 만듭니다.  

#### Promise 사용 예:  
```js  
function fetchData() {  
  fetch("https://api.example.com/data")  
    .then(response => response.json())  
    .then(data => console.log(data))  
    .catch(error => console.error("Error:", error));  
}  
fetchData();  
```  

#### Async/Await 사용 예:  
```js  
async function fetchData() {  
  try {  
    const response = await fetch("https://api.example.com/data");  
    const data = await response.json();  
    console.log(data);  
  } catch (error) {  
    console.error("Error:", error);  
  }  
}  
fetchData();  
```  

---

### 에러 처리: try/catch로 안전한 코드 작성  
`async/await`를 사용하면 `try/catch`를 활용해 예외 상황을 쉽게 처리할 수 있습니다.  
예를 들어, 네트워크 응답 상태를 검사하고 오류를 처리하려면 아래와 같이 작성할 수 있습니다:  

```js  
async function fetchData() {  
  try {  
    const response = await fetch("https://api.example.com/data");  
    if (!response.ok) {  
      throw new Error("Network response was not ok");  
    }  
    const data = await response.json();  
    console.log(data);  
  } catch (error) {  
    console.error("Error:", error);  
  }  
}  

fetchData();  
```  

---

### Promise.all과 Async/Await: 병렬 작업 처리  
여러 비동기 작업을 병렬로 처리해야 할 경우, `Promise.all()`을 활용하면 좋습니다.  
이 함수는 모든 `Promise`가 해결될 때까지 기다린 후 결과를 반환합니다.  

#### 예시:  
```js  
async function fetchMultipleData() {  
  const [response1, response2] = await Promise.all([  
    fetch("https://api.example.com/data1"),  
    fetch("https://api.example.com/data2"),  
  ]);  
  
  const data1 = await response1.json();  
  const data2 = await response2.json();  
  
  console.log(data1, data2);  
}  

fetchMultipleData();  
```  

---

### 비동기 함수의 성능과 가독성  
`async/await`는 비동기 처리를 동기식 코드처럼 작성할 수 있게 해줍니다.  
이는 코드의 **가독성과 유지보수성**을 크게 향상시키지만, 성능 자체를 개선하지는 않습니다.  
즉, 어떤 비동기 작업을 수행하든 백그라운드에서 비동기로 처리되므로 작업 속도에는 차이가 없습니다.  

---

### 요약 및 활용 팁  
- `async` 함수는 항상 `Promise`를 반환합니다.  
- `await`는 `Promise`가 처리될 때까지 대기한 뒤 결과를 반환합니다.  
- 비동기 작업에서 발생할 수 있는 예외는 `try/catch`로 처리하세요.  
- 병렬 작업에는 `Promise.all()`을 사용하여 효율성을 높이세요.  

`async/await`는 JavaScript의 비동기 작업을 더 직관적이고 간단하게 작성할 수 있도록 도와줍니다.  
비동기 작업이 포함된 프로젝트에서 적극적으로 활용해 보세요!  
