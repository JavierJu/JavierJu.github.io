---
title: "JavaScript 함수의 기초부터 응용까지: 완벽 가이드"
excerpt: "JavaScript 함수의 기본 개념부터 매개변수, 반환값, 화살표 함수, 그리고 응용까지 자세히 살펴봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Functions, Programming Basics, Web Development]
permalink: /JavaScript/function-basics-guide/
toc: true
toc_sticky: true
date: 2024-11-22
last_modified_at: 2024-11-22
---

JavaScript에서 함수는 코드 재사용성과 유지보수성을 높여주는 중요한 요소입니다. 이 글에서는 함수의 정의, 매개변수와 반환값, 다양한 함수 스타일, 그리고 응용 사례까지 알아봅니다.

---

## 함수의 기본 구조

JavaScript에서 함수는 아래와 같은 형식으로 정의됩니다.

```js
function 함수이름(매개변수1, 매개변수2, ...) {
    // 실행할 코드
    return 반환값; // (선택사항)
}
```

### 예제: 간단한 함수
```js
function sayHello() {
    console.log("Hello, JavaScript!");
}
sayHello(); // "Hello, JavaScript!" 출력
```

---

## 매개변수와 인수

### 매개변수와 인수란?

- **매개변수(Parameter)**: 함수 정의 시 입력값을 받을 변수.
- **인수(Argument)**: 함수 호출 시 실제로 전달하는 값.

#### 예제: 매개변수와 인수 사용
```js
function greet(name) {
    console.log(`Hello, ${name}!`);
}
greet("Alice"); // "Hello, Alice!" 출력
```

---

## 여러 개의 매개변수

함수는 여러 개의 매개변수를 받을 수 있습니다.

#### 예제: 두 수의 합 구하기
```js
function add(a, b) {
    return a + b;
}
let result = add(5, 3);
console.log(result); // 8 출력
```

---

## 기본값 설정

매개변수에 기본값을 설정할 수도 있습니다.

#### 예제: 기본값 사용
```js
function greet(name = "Guest") {
    console.log(`Hello, ${name}!`);
}
greet();          // "Hello, Guest!" 출력
greet("Charlie"); // "Hello, Charlie!" 출력
```

---

## 반환값

`return` 키워드를 사용하여 값을 반환할 수 있습니다.

#### 예제: 곱셈 함수
```js
function multiply(a, b) {
    return a * b;
}
let product = multiply(4, 5);
console.log(product); // 20 출력
```

`return`이 없는 함수는 자동으로 `undefined`를 반환합니다.

---

## 익명 함수와 화살표 함수

### 익명 함수 (Anonymous Function)

익명 함수는 변수에 저장하거나 즉시 실행할 수 있습니다.

#### 예제: 익명 함수
```js
let square = function (x) {
    return x * x;
};
console.log(square(4)); // 16 출력
```

### 화살표 함수 (Arrow Function)

더 간결한 문법으로 익명 함수를 작성할 수 있습니다.

#### 예제: 화살표 함수
```js
let square = (x) => x * x;
console.log(square(4)); // 16 출력
```

---

## 함수의 응용

### 함수 내부에서 다른 함수 호출

#### 예제: 제곱의 합 구하기
```js
function square(x) {
    return x * x;
}

function sumOfSquares(a, b) {
    return square(a) + square(b);
}

console.log(sumOfSquares(2, 3)); // 13 출력
```

### 콜백 함수 (Callback Function)

콜백 함수는 다른 함수의 인수로 전달되는 함수입니다.

#### 예제: 콜백 함수
```js
function processUserInput(callback) {
    let name = "Alice";
    callback(name);
}

processUserInput(function (name) {
    console.log(`Hello, ${name}!`);
});
// "Hello, Alice!" 출력
```

---

## 함수의 유효 범위 (Scope)

함수 안에서 선언된 변수는 함수 외부에서 접근할 수 없습니다.

#### 예제: 유효 범위
```js
function example() {
    let localVariable = "I'm local";
    console.log(localVariable);
}
example(); // "I'm local" 출력
console.log(localVariable); // 오류: localVariable은 정의되지 않음
```

---

## 예제 프로젝트: 간단한 계산기 함수

마지막으로 간단한 계산기를 만들어 봅시다.

#### 예제: 계산기 함수
```js
function calculator(a, b, operator) {
    if (operator === "+") return a + b;
    if (operator === "-") return a - b;
    if (operator === "*") return a * b;
    if (operator === "/") return b !== 0 ? a / b : "Cannot divide by zero";
    return "Invalid operator";
}

console.log(calculator(10, 5, "+")); // 15
console.log(calculator(10, 5, "-")); // 5
console.log(calculator(10, 5, "*")); // 50
console.log(calculator(10, 5, "/")); // 2
console.log(calculator(10, 0, "/")); // "Cannot divide by zero"
```

---

JavaScript 함수는 매우 유연하고 강력한 도구입니다. 위의 예제를 통해 기초를 다지고, 다양한 프로젝트에서 활용해 보세요!
