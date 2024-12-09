---
title: "JavaScript에서 Promise의 역할과 활용"
excerpt: "JavaScript에서 Promise를 사용하여 비동기 작업을 처리하는 방법과 callback hell 문제를 해결하는 방법에 대해 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Promise, 비동기, Callback Hell, Web Development]
permalink: /JavaScript/promise-example/
toc: true
toc_sticky: true
date: 2024-12-3
last_modified_at: 2024-12-3
---

JavaScript에서 비동기 작업을 처리하는 방법 중 하나인 `Promise`는, 특히 **callback hell** 문제를 해결하는 데 유용하게 사용됩니다. `Promise`를 사용하면 비동기 작업을 더 깔끔하고 가독성 있게 처리할 수 있습니다. 이번 글에서는 `Promise`의 개념, 사용법, 그리고 이를 사용하는 이유와 예제에 대해 자세히 알아보겠습니다.

## 1. Callback Hell 문제

`Callback hell`은 여러 개의 콜백 함수가 중첩되어 가독성이 떨어지고 코드 관리가 어려워지는 문제입니다. 예를 들어, 여러 비동기 작업이 순차적으로 실행되어야 할 때, 각 작업에 콜백을 넣어야 한다면 코드가 깊어지고 복잡해집니다.

### Callback Hell 예제:

``` js
function fetchData(callback) {
  setTimeout(() => {
    console.log("데이터를 가져왔습니다.");
    callback();
  }, 1000);
}

fetchData(function() {
  fetchData(function() {
    fetchData(function() {
      console.log("모든 작업 완료!");
    });
  });
});
```

위 코드는 `setTimeout`을 통해 비동기 작업을 수행하고, 각 작업이 끝날 때마다 콜백 함수를 호출합니다. 그러나 콜백이 중첩되어 코드가 가독성이 떨어지며, 유지보수가 어려워집니다.

## 2. Promise란 무엇인가?

`Promise`는 비동기 작업을 처리하는 객체로, 특정 시점에서 결과를 반환할 수 있는 "약속"을 의미합니다. `Promise`는 3가지 상태를 가질 수 있습니다:
- **Pending**: 대기 중 (작업이 아직 끝나지 않음)
- **Fulfilled**: 이행됨 (작업이 성공적으로 끝났을 때)
- **Rejected**: 거부됨 (작업이 실패했을 때)

`Promise`를 사용하면 비동기 코드가 더 직관적이고 가독성 있게 작성될 수 있습니다.

### Promise 기본 구조:

``` js
let promise = new Promise(function(resolve, reject) {
  // 비동기 작업 수행
  let success = true; // 예시로 성공한 경우
  if(success) {
    resolve("작업 성공!");
  } else {
    reject("작업 실패!");
  }
});

promise.then(function(result) {
  console.log(result);  // "작업 성공!"
}).catch(function(error) {
  console.log(error);   // "작업 실패!"
});
```

## 3. 왜 Promise를 사용하는가?

`Promise`는 `callback hell`을 해결하는 데 매우 유용합니다. `then()`과 `catch()`를 사용하여 비동기 작업을 순차적으로 처리할 수 있으며, 코드 흐름이 직관적이고 평탄해집니다.

### Promise를 사용하는 이유:
- **가독성**: 콜백이 중첩되지 않고, 코드를 읽기 쉽고, 이해하기 쉽게 만듭니다.
- **에러 처리**: `catch()`를 통해 에러를 한 곳에서 처리할 수 있어, 코드에서 에러 처리가 일관됩니다.
- **체이닝(Chaining)**: 여러 개의 비동기 작업을 순차적으로 연결할 수 있습니다.

## 4. Promise 체이닝 (Chaining)

`Promise`는 `.then()`을 사용하여 여러 비동기 작업을 순차적으로 연결할 수 있습니다. 이를 통해 비동기 작업들을 명확하고, 읽기 쉬운 코드로 작성할 수 있습니다.

### Promise 체이닝 예제:

``` js
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("데이터를 가져왔습니다.");
      resolve();
    }, 1000);
  });
}

fetchData()
  .then(() => fetchData())
  .then(() => fetchData())
  .then(() => console.log("모든 작업 완료!"))
  .catch(error => console.log("에러 발생:", error));
```

위 코드에서 `.then()`을 사용하여 순차적으로 비동기 작업을 처리하고 있습니다. 각 `fetchData()`가 끝나면 다음 작업이 진행됩니다. 또한, `catch()`를 사용하여 에러를 처리할 수 있습니다.

## 5. Promise.all()

`Promise.all()`은 여러 개의 비동기 작업을 병렬로 처리하고, 모든 작업이 완료될 때까지 기다립니다. 모든 작업이 성공적으로 완료되면 결과를 배열로 반환하고, 하나라도 실패하면 바로 `catch()`로 에러를 처리할 수 있습니다.

### Promise.all() 예제:

``` js
function fetchData1() {
  return new Promise(resolve => setTimeout(() => resolve("데이터 1"), 1000));
}

function fetchData2() {
  return new Promise(resolve => setTimeout(() => resolve("데이터 2"), 1500));
}

function fetchData3() {
  return new Promise(resolve => setTimeout(() => resolve("데이터 3"), 500));
}

Promise.all([fetchData1(), fetchData2(), fetchData3()])
  .then(results => {
    console.log(results); // ["데이터 1", "데이터 2", "데이터 3"]
  })
  .catch(error => {
    console.log("에러 발생:", error);
  });
```

이 예제에서는 세 개의 `fetchData()`가 병렬로 실행됩니다. 모든 작업이 끝난 후, 결과가 배열로 반환됩니다.

## 6. Promise의 장점과 사용 상황

- **동기적 흐름처럼 비동기 작업을 처리**: `Promise`를 사용하면 비동기 작업을 마치 동기 작업처럼 순차적으로 처리할 수 있습니다.
- **비동기 작업의 오류 처리**: 에러를 `.catch()`로 한 곳에서 처리할 수 있어 코드가 깔끔해집니다.
- **비동기 작업의 병렬 처리**: `Promise.all()`을 사용하면 여러 비동기 작업을 병렬로 실행할 수 있습니다.

### 사용 상황:
- **네트워크 요청 처리**: API 호출과 같은 비동기 작업을 처리할 때 유용합니다.
- **파일 읽기/쓰기**: 파일 시스템에서 비동기 작업을 할 때도 유용합니다.
- **웹 애플리케이션의 사용자 경험 향상**: 비동기 작업을 효율적으로 처리하여 UI가 멈추지 않게 합니다.

## 7. Async/Await와의 관계

`Promise`는 `async/await`와 함께 사용될 때 더욱 직관적이고 간결한 코드를 작성할 수 있습니다. `async` 함수 내에서 `await`를 사용하면 `Promise`의 이행을 기다릴 수 있습니다.

### Async/Await 예제:

``` js
async function fetchData() {
  let result1 = await fetchData1();
  let result2 = await fetchData2();
  console.log(result1, result2);
}

fetchData();
```

`async/await`를 사용하면 `then()` 체이닝보다 더 동기적인 흐름처럼 코드를 작성할 수 있습니다.

---

`Promise`는 비동기 작업을 처리하는 데 있어 매우 중요한 도구입니다. `callback hell` 문제를 해결하고, 여러 비동기 작업을 병렬 또는 순차적으로 처리할 수 있게 도와줍니다. `Promise`를 활용하면 코드가 더 직관적이고, 가독성 있게 작성될 수 있습니다.
