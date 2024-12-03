---
title: "JavaScript의 if와 throw: 역할과 차이점"
excerpt: "JavaScript에서 if와 throw의 개념, 역할, 그리고 차이점을 코드 예제와 함께 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Control Flow, Error Handling, Programming Basics]
permalink: /JavaScript/if-throw-difference/
toc: true
toc_sticky: true
date: 2024-12-3
last_modified_at: 2024-12-3
---

JavaScript에서 `if`와 `throw`는 제어 흐름을 다루는 중요한 키워드입니다. 각각의 역할과 차이점을 코드 예제와 함께 알아보겠습니다.

## `if`의 역할
`if`는 특정 조건이 참인지 확인하고, 조건에 따라 실행할 코드를 결정하는 조건문입니다. `else`와 결합하면 참일 때와 거짓일 때의 실행 흐름을 나눌 수 있습니다.

### 기본 예제
```js
let x = 10;

if (x > 5) {
  console.log("x는 5보다 큽니다.");
} else {
  console.log("x는 5보다 작거나 같습니다.");
}
```
- `if`는 조건이 참일 경우 첫 번째 블록의 코드를 실행합니다.
- `else`는 조건이 거짓일 경우 대체 실행 블록을 제공합니다.

---

## `throw`의 역할
`throw`는 예외를 발생시키는 데 사용됩니다. `throw`를 통해 에러 객체나 메시지를 던지면, 프로그램은 해당 시점에서 실행을 멈추고 예외 처리를 위한 흐름으로 이동합니다.

### 기본 예제
```js
function checkAge(age) {
  if (age < 18) {
    throw new Error("나이가 18세 미만입니다.");
  } else {
    console.log("입장 가능합니다.");
  }
}

try {
  checkAge(15);
} catch (e) {
  console.log(e.message); // "나이가 18세 미만입니다."
}
```
- `throw`는 조건을 만족하면 에러를 던집니다.
- `try...catch`를 사용해 예외를 처리할 수 있습니다.
- 예외가 발생하면 나머지 코드는 실행되지 않고, 흐름이 `catch`로 넘어갑니다.

---

## `else`와 `throw`의 차이점
`else`와 `throw`는 제어 흐름을 다루는 방식에서 근본적인 차이가 있습니다.

| **특징** | **else** | **throw** |
|----------|----------|-----------|
| **역할** | 조건이 거짓일 때 대체 실행 흐름 제공 | 예외를 발생시켜 실행 중단 |
| **제어 흐름** | 정상적인 흐름 유지 | 정상 흐름 중단 및 예외 처리로 전환 |
| **사용 예시** | 조건에 따라 여러 옵션을 처리할 때 | 에러를 감지하고 처리하거나 프로그램 중단이 필요할 때 |

---

## 요약
- `if`는 조건을 평가하여 실행 흐름을 제어합니다.
- `throw`는 예외를 발생시켜 프로그램의 흐름을 예외 처리 단계로 이동시킵니다.
- `else`는 대체 실행 흐름을 제공하지만, `throw`는 오류 상황에서 실행을 중단합니다.

두 키워드는 모두 상황에 맞게 적절히 사용해야 코드의 가독성과 안정성이 높아집니다.
