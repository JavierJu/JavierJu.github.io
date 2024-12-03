---
title: "JavaScript 매개변수 완벽 가이드: 기본부터 고급까지"
excerpt: "JavaScript 함수 매개변수의 기본 개념, 나머지 매개변수, 기본값 설정, 구조 분해 등 다양한 활용법을 정리합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Functions, Parameters, ES6, Programming]
permalink: /JavaScript/parameters-complete-guide/
toc: true
toc_sticky: true
date: 2024-11-27
last_modified_at: 2024-11-27
---

JavaScript에서 매개변수는 함수의 동작을 유연하게 제어하는 중요한 요소입니다. 이 글에서는 매개변수의 기초 개념부터 고급 사용법까지를 단계별로 알아보겠습니다.

## 매개변수(Parameter)와 인수(Argument)
- **매개변수(Parameter):** 함수 정의 시 사용하는 변수입니다.
- **인수(Argument):** 함수 호출 시 매개변수에 전달되는 값입니다.

```js
function greet(name) {
    console.log(`Hello, ${name}!`);
}
greet('Alice'); // Hello, Alice!
```

## 매개변수의 기본값 (Default Parameters)
JavaScript에서는 매개변수에 기본값을 설정하여, 인수가 전달되지 않더라도 기본값이 사용되도록 할 수 있습니다.

```js
function greet(name = 'Guest') {
    console.log(`Hello, ${name}!`);
}
greet(); // Hello, Guest!
greet('Alice'); // Hello, Alice!
```

## 매개변수와 인수의 개수 차이
함수에 전달된 인수의 개수는 매개변수의 개수와 일치하지 않아도 에러가 발생하지 않습니다.
- **인수가 부족한 경우:** 전달되지 않은 매개변수는 `undefined`로 설정됩니다.
- **인수가 초과된 경우:** 초과된 인수는 무시됩니다.

```js
function greet(name, age) {
    console.log(`${name} is ${age} years old.`);
}
greet('Alice'); // Alice is undefined years old.
greet('Alice', 25, 'extra'); // Alice is 25 years old.
```

## `arguments` 객체
전통적인 함수에서는 `arguments` 객체를 사용해 전달된 모든 인수에 접근할 수 있습니다. 이 객체는 배열처럼 작동하지만, 배열이 아닙니다.

```js
function sum() {
    let total = 0;
    for (let i = 0; i < arguments.length; i++) {
        total += arguments[i];
    }
    return total;
}
console.log(sum(1, 2, 3, 4)); // 10
```

> 화살표 함수에는 `arguments` 객체가 없으므로 사용하려면 전통적인 함수 문법을 사용해야 합니다.

## 나머지 매개변수 (Rest Parameters)
ES6에서 도입된 나머지 매개변수를 사용하면 가변 길이의 인수를 배열로 받을 수 있습니다.

```js
function sum(...numbers) {
    return numbers.reduce((acc, cur) => acc + cur, 0);
}
console.log(sum(1, 2, 3, 4)); // 10
```

## 매개변수 구조 분해 (Destructuring)
매개변수로 객체나 배열을 받을 때 구조 분해를 활용하면 더 간결하고 가독성 높은 코드를 작성할 수 있습니다.

```js
// 객체 구조 분해
function printUser({ name, age }) {
    console.log(`${name} is ${age} years old.`);
}
const user = { name: 'Alice', age: 25 };
printUser(user); // Alice is 25 years old.

// 배열 구조 분해
function printCoordinates([x, y]) {
    console.log(`X: ${x}, Y: ${y}`);
}
printCoordinates([10, 20]); // X: 10, Y: 20
```

## 매개변수의 타입 검증
JavaScript는 동적 타입 언어이므로, 함수 내부에서 매개변수의 타입을 검증하는 것이 일반적입니다.

```js
function multiply(a, b) {
    if (typeof a !== 'number' || typeof b !== 'number') {
        throw new Error('Both arguments must be numbers');
    }
    return a * b;
}
console.log(multiply(2, 3)); // 6
// console.log(multiply(2, 'x')); // Error: Both arguments must be numbers
```

## 콜백 함수 매개변수
함수는 다른 함수의 매개변수로 전달될 수 있습니다. 이를 활용해 고차 함수(high-order function)를 작성할 수 있습니다.

```js
function processArray(arr, callback) {
    return arr.map(callback);
}
const numbers = [1, 2, 3];
const squared = processArray(numbers, num => num * num);
console.log(squared); // [1, 4, 9]
```

## 결론
JavaScript의 매개변수는 단순한 값 전달 이상으로 강력한 기능을 제공합니다. 기본값, 나머지 매개변수, 구조 분해 등 다양한 방법을 활용하여 함수의 가독성과 재사용성을 높일 수 있습니다. 이 글을 통해 JavaScript 매개변수의 기초부터 고급 사용법까지 이해하셨기를 바랍니다.
