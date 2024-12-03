---
title: "JavaScript의 Call Stack, Web API, 단일 스레드: 실행 원리와 비동기 처리"
excerpt: "JavaScript의 단일 스레드 환경에서 Call Stack, Web API, Event Loop가 상호작용하는 방식과 비동기 처리 과정을 자세히 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Web API, 비동기 처리, Event Loop, Call Stack]
permalink: /JavaScript/call-stack-web-api-event-loop/
toc: true
toc_sticky: true
date: 2024-12-2
last_modified_at: 2024-12-2
---

JavaScript는 단일 스레드로 동작하면서도 비동기 작업을 효과적으로 처리할 수 있는 언어입니다. 이 글에서는 **Call Stack**, **Web API**, **단일 스레드**, 그리고 **Event Loop**가 어떻게 동작하는지 전체적으로 설명합니다.

---

## JavaScript의 단일 스레드

JavaScript는 **단일 스레드(single-threaded)**로 동작합니다.  
단일 스레드란 한 번에 하나의 작업만 처리할 수 있다는 의미로, 코드 실행 순서가 예측 가능하다는 장점이 있습니다.  

하지만 한 번에 하나의 작업만 처리하므로, 복잡한 작업이 오래 걸리면 프로그램이 멈춘 것처럼 보일 수 있습니다(블로킹). 이를 해결하기 위해 JavaScript는 **비동기 처리**를 지원합니다.

---

## Call Stack (호출 스택)

### Call Stack의 정의
**Call Stack**은 JavaScript 엔진이 함수 호출과 실행을 관리하기 위해 사용하는 데이터 구조입니다.  
이 구조는 **후입선출(LIFO: Last In, First Out)** 방식으로 동작합니다.

### Call Stack의 동작 과정
1. **함수 호출** 시, 호출된 함수가 스택에 쌓입니다.
2. **함수 실행 완료** 시, 해당 함수는 스택에서 제거됩니다.
3. 스택이 비어 있으면 새로운 작업을 처리할 준비가 된 상태입니다.

### Call Stack 예제

```js
function first() {
  console.log("First function");
  second();
}

function second() {
  console.log("Second function");
}

first();
```
**Call Stack 흐름**:
1. `first()` 호출 → 스택에 추가.
2. `console.log("First function")` 실행 후 제거.
3. `second()` 호출 → 스택에 추가.
4. `console.log("Second function")` 실행 후 제거.
5. `second()` 완료 → 스택에서 제거.
6. `first()` 완료 → 스택에서 제거.

---

## Web API와 비동기 처리

### Web API란?
JavaScript는 비동기 작업을 실행하기 위해 브라우저에서 제공하는 **Web API**를 사용합니다.  
Web API는 타이머(`setTimeout`), Ajax 요청(`fetch`), DOM 이벤트 등 다양한 비동기 작업을 처리합니다.

### 비동기 처리 흐름
1. **Call Stack에서 비동기 함수 호출**  
   비동기 작업(예: `setTimeout`)은 Call Stack에서 Web API로 전달됩니다.
2. **Web API에서 작업 처리**  
   Web API는 비동기 작업을 실행하며, 완료되면 작업 결과를 **Task Queue**에 추가합니다.
3. **Task Queue와 Event Loop**  
   Event Loop는 Call Stack이 비어 있을 때 Task Queue의 작업을 가져와 실행합니다.

---

## Event Loop와 Task Queue

### Event Loop란?
**Event Loop**는 Call Stack과 Task Queue를 관리하며, JavaScript가 비동기 작업을 처리할 수 있게 하는 핵심 메커니즘입니다.

1. **Call Stack이 비었는지 확인**합니다.
2. Task Queue에서 대기 중인 작업을 가져와 실행합니다.

### Microtask와 Macrotask
- **Microtask Queue**  
  - `Promise.then`, `queueMicrotask` 등 작업이 여기에 추가됩니다.
  - Microtask는 항상 Macrotask보다 **우선** 실행됩니다.
  
- **Macrotask Queue**  
  - `setTimeout`, `setInterval`, DOM 이벤트 등이 여기에 추가됩니다.
  - Microtask Queue가 비어야 Macrotask가 실행됩니다.

---

## 실행 예제

### 코드 예제
```js
console.log("Start");

setTimeout(() => {
  console.log("Timeout Callback");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise Callback");
});

console.log("End");
```
### 실행 결과
```sql
Start 
End 
Promise Callback 
Timeout Callback
```

### 실행 흐름
1. `console.log("Start")` → Call Stack에서 실행 후 제거.
2. `setTimeout` 호출 → Web API로 전달 후 Call Stack에서 제거.
3. `Promise.resolve().then()` → Microtask Queue에 추가.
4. `console.log("End")` → Call Stack에서 실행 후 제거.
5. **Microtask 실행** → "Promise Callback" 출력.
6. **Macrotask 실행** → "Timeout Callback" 출력.

---

## 정리

JavaScript의 비동기 처리 방식은 **단일 스레드** 환경에서 효율적으로 동작하도록 설계되었습니다.  
1. Call Stack은 함수 실행을 관리합니다.  
2. Web API는 비동기 작업을 처리합니다.  
3. Event Loop는 Task Queue를 관리하며 Call Stack이 비었을 때 대기 중인 작업을 실행합니다.  
4. Microtask는 항상 Macrotask보다 우선 처리됩니다.

이 구조를 이해하면, JavaScript의 비동기 동작 방식을 효과적으로 활용할 수 있습니다.
