---
title: "JavaScript에서 콜백 지옥 해결하기"
excerpt: "JavaScript에서 콜백 지옥이란 무엇인지, 그리고 이를 해결할 수 있는 방법에 대해 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Async, Promise, Callback]
permalink: /JavaScript/callback-hell/
toc: true
toc_sticky: true
date: 2024-12-2
last_modified_at: 2024-12-2
---

JavaScript에서 콜백 지옥(callback hell)은 비동기 처리를 위한 콜백 함수들이 중첩되면서 코드가 복잡해지고 가독성이 떨어지는 현상을 의미합니다. 이러한 문제를 해결하기 위한 다양한 방법들을 소개하겠습니다.

## 콜백 지옥이란?

콜백 지옥은 여러 비동기 작업을 순차적으로 처리할 때, 콜백 함수 안에 또 다른 콜백을 넣어야 하는 구조에서 발생합니다. 이로 인해 코드가 중첩되며 가독성 문제가 생기고, 디버깅과 유지보수가 어려워질 수 있습니다. 예를 들어, 아래와 같은 코드를 생각해봅시다.

```js
getData(function(result1) {
    processData(result1, function(result2) {
        saveData(result2, function(result3) {
            console.log('모든 작업 완료:', result3);
        });
    });
});
```

위 코드는 함수가 중첩되면서 가독성이 떨어지며, 오류가 발생했을 때 처리하기도 어려워집니다.

## 콜백 지옥 개선 방법

### 1. 이름 있는 함수 사용

익명 함수 대신 이름 있는 함수를 사용하면 코드의 가독성이 높아집니다. 또한, 중첩된 함수들을 별도의 함수로 분리함으로써 더 명확하게 작업을 처리할 수 있습니다.

```js
function handleGetData(result1) {
    processData(result1, handleProcessData);
}

function handleProcessData(result2) {
    saveData(result2, handleSaveData);
}

function handleSaveData(result3) {
    console.log('모든 작업 완료:', result3);
}

getData(handleGetData);
```

하지만 여전히 콜백을 사용하고 있기 때문에 코드가 복잡해질 수 있습니다.

### 2. Promise 사용

JavaScript의 `Promise` 객체를 사용하면 콜백 대신 `then` 체인을 이용해 비동기 작업을 순차적으로 처리할 수 있습니다. 예를 들어, 다음과 같이 개선할 수 있습니다.

```js
getData()
    .then(result1 => processData(result1))
    .then(result2 => saveData(result2))
    .then(result3 => console.log('모든 작업 완료:', result3))
    .catch(error => console.error('오류 발생:', error));
```

이 방법은 콜백 지옥을 피할 수 있으며, 예외 처리가 간편해집니다.

### 3. Async/Await 사용

`async`와 `await` 키워드를 사용하면 비동기 코드를 마치 동기 코드처럼 작성할 수 있습니다. `Promise` 기반 함수에서 작동하며, 가독성을 더욱 개선할 수 있습니다.

```js
async function process() {
    try {
        const result1 = await getData();
        const result2 = await processData(result1);
        const result3 = await saveData(result2);
        console.log('모든 작업 완료:', result3);
    } catch (error) {
        console.error('오류 발생:', error);
    }
}

process();
```

`async/await`는 비동기 코드를 동기식 코드처럼 읽을 수 있게 만들어 가독성을 크게 향상시킵니다.

### 4. 라이브러리 사용

비동기 작업이 많고 복잡한 경우에는 `RxJS`와 같은 라이브러리를 사용할 수 있습니다. 특히, 스트림 기반의 작업을 처리할 때 유용합니다.

```js
import { from } from 'rxjs';
import { map, switchMap } from 'rxjs/operators';

from(getData())
    .pipe(
        switchMap(result1 => processData(result1)),
        switchMap(result2 => saveData(result2))
    )
    .subscribe({
        next: result3 => console.log('모든 작업 완료:', result3),
        error: error => console.error('오류 발생:', error)
    });
```

`RxJS`는 비동기 흐름을 더 쉽게 처리할 수 있게 도와줍니다.

## 언제 어떤 방법을 사용할까?

- **단순한 비동기 처리:** `Promise`나 `async/await`가 적합합니다.
- **비동기 작업이 많고 복잡한 경우:** `RxJS`와 같은 라이브러리를 사용하는 것이 좋습니다.
- **기존 코드와 호환성을 유지해야 할 경우:** 콜백을 사용하면서도 `Promise`로 리팩터링을 고려할 수 있습니다.

이 방법들을 통해 콜백 지옥에서 벗어나 더 깔끔하고 유지보수하기 쉬운 코드를 작성할 수 있습니다.
