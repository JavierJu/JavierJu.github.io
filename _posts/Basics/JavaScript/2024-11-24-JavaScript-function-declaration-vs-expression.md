---
title: "JavaScript 함수 선언과 함수 표현식: 차이점과 사용 가이드"
excerpt: "JavaScript에서 함수 선언과 함수 표현식의 차이점, 각각의 장단점, 그리고 상황별 사용 가이드를 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Function Declaration, Function Expression, Hoisting, Debugging]
permalink: /JavaScript/function-declaration-vs-expression/
toc: true
toc_sticky: true
date: 2024-11-24
last_modified_at: 2024-11-24
---

JavaScript에서는 함수를 정의하는 방법으로 함수 선언과 함수 표현식이 있습니다. 두 방식 모두 자주 사용되지만, **호이스팅** 동작, 유연성, 디버깅 측면에서 차이가 있습니다. 이번 글에서는 두 방식의 특징과 장단점, 그리고 어떤 상황에서 적합한지 살펴보겠습니다.

---

## 함수 선언 (Function Declaration)

```js
function sayHello() {
    console.log("Hello!");
}
```

### 특징
- `function` 키워드로 시작하며 **이름이 필수적**입니다.
- **호이스팅**(hoisting)이 적용되어, 함수 선언부 이전에도 호출이 가능합니다.

```js
sayHello(); // "Hello!"

function sayHello() {
    console.log("Hello!");
}
```

- 독립적으로 선언된 함수이므로 코드의 가독성이 높고, 전역적으로 호출할 때 적합합니다.

---

## 함수 표현식 (Function Expression)

```js
const sayHello = function() {
    console.log("Hello!");
};
```

### 특징
- 함수를 변수에 할당하여 정의합니다.
- **호이스팅이 적용되지 않습니다.** 변수에 함수가 할당되기 전에는 사용할 수 없습니다.

```js
sayHello(); // ReferenceError: Cannot access 'sayHello' before initialization

const sayHello = function() {
    console.log("Hello!");
};
```

- 익명 함수 또는 이름이 있는 함수(named function expression)로 정의할 수 있습니다.

```js
const sayHello = function greet() {
    console.log("Hello!");
};
```

---

## 주요 차이점 비교

| **특징**                  | **함수 선언**                                           | **함수 표현식**                                   |
|---------------------------|---------------------------------------------------------|-------------------------------------------------|
| **호이스팅**              | 함수 선언부가 코드 최상단으로 끌어올려짐                  | 변수 선언만 호이스팅되며, 함수는 초기화 후 사용 가능 |
| **익명 함수 사용 여부**   | 불가능 (항상 이름 필요)                                 | 가능 (익명 함수 사용 가능)                       |
| **구문 유연성**           | 독립적으로 사용 가능                                     | 반드시 변수나 다른 표현식의 일부로 사용됨         |
| **디버깅 편의성**         | 함수 이름이 항상 있어서 디버깅이 쉬움                     | 익명 함수는 디버깅이 어려울 수 있음              |

---

## 장단점 비교

### 함수 선언의 장점
- **호이스팅** 덕분에 코드 작성 순서와 관계없이 호출 가능.
- 명료하고 직관적이므로, 코드의 가독성이 좋음.

### 함수 선언의 단점
- 호출 시점의 자유도가 너무 높아, 복잡한 코드에서는 흐름을 혼동할 수 있음.

### 함수 표현식의 장점
- 함수의 정의와 호출 시점을 명확히 제어할 수 있음.
- **콜백 함수**로 유용하며, 특정 컨텍스트 내에서만 사용되는 함수에 적합.

### 함수 표현식의 단점
- 초기화 전에 호출 시 `ReferenceError` 발생 가능.
- 익명 함수의 경우, 디버깅 시 함수 이름이 표시되지 않아 어려울 수 있음.

---

## 주로 쓰이는 상황

### 함수 선언
- 전역적으로 호출될 필요가 있는 함수.
- 주요 로직이나 여러 곳에서 반복적으로 사용할 함수 정의에 적합.

```js
function calculateSum(a, b) {
    return a + b;
}
console.log(calculateSum(3, 5)); // 8
```

### 함수 표현식
- **콜백 함수**로 자주 사용되며, 특정 시점에만 사용되는 함수 정의에 적합.

```js
// 콜백 함수로 사용
setTimeout(function() {
    console.log("3초 후 실행");
}, 3000);

// 즉시 실행 함수
(function() {
    console.log("즉시 실행!");
})();
```

---

## 결론: 언제 어떤 방식을 선택할까?

- **함수 선언**은 코드의 가독성을 높이고, 함수 호출 시점의 제약이 적어 일반적인 함수 정의에 적합합니다.
- **함수 표현식**은 실행 순서를 명확히 하고, 콜백 함수나 제한적인 컨텍스트 내에서만 사용하는 경우 적합합니다.
- 실제 프로젝트에서는 상황에 따라 두 방식을 조화롭게 사용하는 것이 중요합니다.
