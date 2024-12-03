---
title: "Promise와 AJAX: 관계와 활용법"
excerpt: "JavaScript의 비동기 처리를 위한 Promise와 AJAX의 관계를 이해하고, 효율적인 비동기 요청 처리를 위한 활용법을 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Promise, AJAX, Web Development, Asynchronous]
permalink: /JavaScript/promise-and-ajax/
toc: true
toc_sticky: true
date: 2024-12-3
last_modified_at: 2024-12-3
---

## Promise와 AJAX의 관계

JavaScript에서 비동기 처리를 위한 두 가지 주요 개념인 **Promise**와 **AJAX**는 밀접한 관련이 있습니다. AJAX 요청을 Promise로 처리하면 비동기 작업의 가독성과 유지보수성이 크게 향상됩니다. 본 문서에서는 이 둘의 관계와 활용법을 살펴보겠습니다.

---

### AJAX란?

AJAX(Asynchronous JavaScript and XML)는 웹 페이지를 새로 고치지 않고 서버와 데이터를 주고받는 기술입니다. 이를 통해 빠른 사용자 경험을 제공할 수 있습니다. AJAX는 주로 `XMLHttpRequest` 객체나 현대적인 `fetch` API를 사용하여 구현됩니다.

---

### Promise란?

**Promise**는 비동기 작업의 완료 상태를 나타내는 JavaScript 객체로, 작업 성공(`resolve`)과 실패(`reject`)를 명확히 처리할 수 있도록 합니다. 다음과 같은 상태를 가집니다:
- **Pending**: 작업이 아직 완료되지 않은 초기 상태.
- **Fulfilled**: 작업이 성공적으로 완료된 상태.
- **Rejected**: 작업이 실패한 상태.

---

### AJAX에서 Promise의 역할

AJAX 요청은 비동기 작업이므로, 요청 결과가 반환되기 전까지 코드 실행이 차단되지 않습니다. Promise를 사용하면 AJAX 요청의 성공 및 실패 결과를 더 직관적으로 처리할 수 있습니다.

---

### `XMLHttpRequest`와 Promise

`XMLHttpRequest`는 기본적으로 콜백을 사용하여 작업을 처리하지만, Promise로 감싸면 코드가 더 읽기 쉬워집니다. 아래는 Promise를 사용해 `XMLHttpRequest`를 처리하는 예제입니다.

``` js
function getData(url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url, true);
    xhr.onload = () => {
      if (xhr.status === 200) {
        resolve(xhr.responseText);
      } else {
        reject(new Error('Failed to fetch data'));
      }
    };
    xhr.onerror = () => reject(new Error('Network error'));
    xhr.send();
  });
}

getData('https://api.example.com/data')
  .then(response => console.log('Response:', response))
  .catch(error => console.log('Error:', error));
```

위 코드에서는 AJAX 요청 성공 시 `resolve`가 호출되고, 실패 시 `reject`가 호출됩니다.

---

### `fetch` API와 Promise

`fetch` API는 최신 브라우저에서 제공하는 간단한 AJAX 요청 방법으로, 기본적으로 Promise를 반환합니다. 별도로 Promise를 작성할 필요 없이 `then()`과 `catch()` 체인을 사용해 결과를 처리할 수 있습니다.

#### `fetch` 예제

``` js
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      return Promise.reject('Failed to load');
    }
    return response.json();
  })
  .then(data => console.log('Data:', data))
  .catch(error => console.log('Error:', error));
```

위 예제는 `fetch` 요청의 성공과 실패를 처리하는 방법을 보여줍니다. `response.ok`를 사용해 상태 코드를 확인하고, JSON 데이터를 변환해 결과를 출력합니다.

---

### Promise를 직접 작성해야 하나요?

- **작성할 필요 없음**: `fetch`와 같은 최신 API는 Promise를 기본적으로 반환하므로, 직접 작성할 필요가 없습니다.
- **직접 작성해야 하는 경우**: 이전 방식인 `XMLHttpRequest`나 사용자 정의 비동기 작업을 Promise로 처리하려면 직접 작성해야 합니다.

---

### 결론

Promise와 AJAX는 함께 사용될 때 강력한 비동기 처리 도구가 됩니다. Promise는 복잡한 콜백 패턴을 단순화하고, AJAX 요청의 결과를 더 직관적으로 처리할 수 있게 도와줍니다. 최신 프로젝트에서는 기본적으로 `fetch` API와 Promise를 함께 사용하여 효율적이고 가독성 높은 코드를 작성하는 것이 좋습니다.
