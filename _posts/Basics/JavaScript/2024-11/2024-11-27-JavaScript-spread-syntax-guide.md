---
title: "JavaScript 스프레드 구문: 활용 방법과 실용 예제"
excerpt: "JavaScript의 스프레드 구문(...)을 사용하여 배열, 객체, 문자열을 다루는 방법과 실용적인 예제를 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Syntax, Arrays, Objects, ES6]
permalink: /JavaScript/spread-syntax-guide/
toc: true
toc_sticky: true
date: 2024-11-27
last_modified_at: 2024-11-27
---

## JavaScript 스프레드 구문이란?

JavaScript의 **스프레드 구문(Spread Syntax)**은 `...`을 사용하여 배열, 객체, 문자열 등 이터러블(iterable) 또는 객체를 개별 요소로 분리하여 다룰 수 있도록 해주는 기능입니다. 코드의 간결성을 높이고, 데이터 처리에서 강력한 유틸리티를 제공합니다.

---

## 배열에서의 스프레드 구문

### 1. 배열 복사
스프레드 구문은 배열의 내용을 쉽게 복사할 수 있습니다.

```js
const arr1 = [1, 2, 3];
const arr2 = [...arr1]; // arr1의 내용을 복사
console.log(arr2); // [1, 2, 3]
```

위 코드는 **얕은 복사(shallow copy)**를 수행하므로, 중첩된 배열이나 객체는 참조만 복사됩니다.

### 2. 배열 합치기
여러 배열을 간단히 합칠 수도 있습니다.

```js
const arr1 = [1, 2];
const arr2 = [3, 4];
const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4]
```

---

## 객체에서의 스프레드 구문

### 1. 객체 복사
객체의 모든 프로퍼티를 복사합니다.

```js
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1 };
console.log(obj2); // { a: 1, b: 2 }
```

### 2. 객체 병합
두 객체를 병합할 때 스프레드 구문을 사용할 수 있습니다.

```js
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };
const merged = { ...obj1, ...obj2 };
console.log(merged); // { a: 1, b: 3, c: 4 }
```

> 주의: 동일한 키가 있을 경우, 뒤쪽 객체의 값이 덮어씁니다.

---

## 함수 호출에서의 스프레드 구문

배열이나 이터러블의 요소를 함수 인수로 간단히 전달할 수 있습니다.

```js
function sum(a, b, c) {
  return a + b + c;
}

const values = [1, 2, 3];
console.log(sum(...values)); // 6
```

`Math.max` 같은 내장 함수에도 유용합니다.

```js
const numbers = [5, 10, 15];
console.log(Math.max(...numbers)); // 15
```

---

## 문자열에서의 스프레드 구문

문자열을 개별 문자로 나눌 수 있습니다.

```js
const str = "Hello";
const chars = [...str];
console.log(chars); // ['H', 'e', 'l', 'l', 'o']
```

---

## 스프레드 구문과 구조 분해의 차이

스프레드 구문은 전체를 복사하거나 병합하는 데 사용되지만, 구조 분해는 개별 요소를 추출합니다.

```js
const arr = [1, 2, 3];

// 구조 분해
const [first, ...rest] = arr;
console.log(first); // 1
console.log(rest); // [2, 3]

// 스프레드 구문
const copied = [...arr];
console.log(copied); // [1, 2, 3]
```

---

## 스프레드 구문의 주의점

1. **얕은 복사**: 중첩된 객체나 배열의 경우, 내부 값은 참조만 복사됩니다.
2. **순서 유지**: 배열의 경우 순서가 유지됩니다. 특정 순서 변경이 필요하다면 추가 작업이 필요합니다.

---

## 마치며

JavaScript의 스프레드 구문은 데이터 구조를 다루는 데 있어 **간결하고 직관적**인 코드를 작성할 수 있게 해줍니다. 배열, 객체, 함수 인수 등 다양한 곳에서 활용할 수 있으니 익숙해져 보세요!
