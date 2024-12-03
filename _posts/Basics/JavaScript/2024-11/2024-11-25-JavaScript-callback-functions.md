---
title: "JavaScript에서 콜백 함수란? 개념과 활용법"
excerpt: "JavaScript의 콜백 함수 개념부터 활용법, 그리고 비동기 프로그래밍에서의 중요성을 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Async Programming, Callback, 비동기, Promise]
permalink: /JavaScript/callback-functions/
toc: true
toc_sticky: true
date: 2024-11-25
last_modified_at: 2024-11-25
---

## 콜백 함수란?

JavaScript에서 **콜백 함수**는 다른 함수의 **인자로 전달되는 함수**를 의미합니다. 특정 작업이 완료된 후 호출되거나, 비동기 작업에서 결과를 처리하기 위해 사용됩니다. 콜백 함수는 JavaScript에서 매우 중요하며, 특히 비동기 프로그래밍에서 많이 활용됩니다.

---

## 콜백 함수의 특징

1. **함수 자체가 값처럼 동작**  
   JavaScript에서는 함수도 객체처럼 다룰 수 있어, 변수에 저장하거나 다른 함수의 인자로 전달할 수 있습니다.

2. **필요할 때 호출**  
   콜백 함수는 전달된 함수가 즉시 실행되는 것이 아니라, 지정된 작업이 완료되었을 때 호출됩니다.

3. **비동기 작업에 사용**  
   데이터 요청, 파일 읽기, 타이머 등 시간이 걸리는 작업을 수행할 때 주로 사용됩니다.

---

## 콜백 함수 사용 예제

### 1. 동기적 콜백 함수 예제

```js
function greet(name, callback) {
    console.log(`Hello, ${name}!`);
    callback();
}

function sayGoodbye() {
    console.log("Goodbye!");
}

// 'greet' 함수에 콜백으로 'sayGoodbye' 전달
greet("Alice", sayGoodbye);
```
**출력:**
```scss
Hello, Alice! Goodbye!
```

---

### 2. 비동기 작업에서의 활용

```js
function fetchData(callback) {
    console.log("Fetching data...");

    // 비동기적으로 데이터를 가져오는 가정
    setTimeout(() => {
        const data = { id: 1, name: "Sample Data" };
        callback(data); // 작업 완료 후 콜백 호출
    }, 2000); // 2초 후 실행
}

function processData(data) {
    console.log("Data received:", data);
}

// fetchData 함수에 콜백 전달
fetchData(processData);
```
**출력:**
```scss
Fetching data... 
//(Data가 준비되기까지 약 2초 후) 
Data received: 
{ 
   id: 1, 
   name: "Sample Data" 
}
```


---

## 콜백 함수의 한계와 대안

### 콜백 헬 (Callback Hell)

콜백을 중첩해서 사용하면 코드가 읽기 어렵고 관리하기 힘든 "콜백 헬" 문제가 발생할 수 있습니다.

```js
fetchData(function (data) {
    process(data, function (result) {
        save(result, function () {
            console.log("완료!");
        });
    });
});
```

### 대안: Promise와 async/await

#### Promise로 변환
```js
function fetchData() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({ id: 1, name: "Sample Data" });
        }, 2000);
    });
}

fetchData()
    .then((data) => {
        console.log("Data received:", data);
    });
```

#### async/await로 변환
```js
async function getData() {
    const data = await fetchData();
    console.log("Data received:", data);
}

getData();
```

---

## 콜백 함수 요약

- **콜백 함수란**: 함수에 전달되어 특정 조건에서 호출되는 함수입니다.
- **언제 사용?**: 주로 비동기 작업에서 작업 완료 후 처리해야 할 로직을 실행할 때.
- **주의사항**: 지나치게 중첩된 콜백 코드는 읽기 어렵고 유지보수하기 힘드므로 Promise나 async/await 사용을 고려하세요.

콜백 함수와 비동기 프로그래밍에 대한 더 깊은 질문이 있다면 댓글로 알려주세요!
