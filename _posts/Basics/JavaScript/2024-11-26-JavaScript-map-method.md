---
title: "JavaScript의 Map 메서드 이해하기"
excerpt: "배열의 각 요소를 변환하는 JavaScript의 `map` 메서드를 활용한 다양한 예제와 사용법을 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Array, Method, Programming]
permalink: /JavaScript/map-method/
toc: true
toc_sticky: true
date: 2024-11-26
last_modified_at: 2024-11-26
---

## JavaScript의 Map 메서드란?

JavaScript에서 `map` 메서드는 **배열**과 같은 반복 가능한 객체에서 각 요소를 변환하여 새로운 배열을 반환하는 데 사용됩니다. 이 메서드는 주로 배열의 각 항목에 대해 **지정된 함수를 실행**하고, 그 결과를 **새로운 배열**로 반환합니다.

### `map` 메서드의 기본 문법

```js
array.map(callback(currentValue, index, array), thisArg);
```

#### 파라미터:
1. **callback**: 배열의 각 항목에 대해 호출되는 함수입니다. 이 함수는 세 개의 인자를 받습니다:
   - `currentValue`: 현재 처리 중인 요소의 값.
   - `index` (선택적): 현재 처리 중인 요소의 인덱스.
   - `array` (선택적): `map` 메서드를 호출한 원본 배열.
2. **thisArg** (선택적): `callback` 함수 내에서 사용할 `this` 값.

### 반환값:
- `map` 메서드는 **새로운 배열**을 반환합니다. 이 배열의 각 항목은 `callback` 함수의 결과로 변환된 값입니다.

## 예시 1: 배열 값 변환

배열의 각 숫자에 2를 곱한 값을 새로운 배열로 반환하는 예시입니다.

```js
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(num => num * 2);
console.log(doubled);  // [2, 4, 6, 8, 10]
```

## 예시 2: 객체 배열에서 특정 속성 추출

객체 배열에서 각 객체의 특정 속성 값을 추출하여 새로운 배열을 만드는 예시입니다.

```js
const users = [
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 30 },
  { name: 'Charlie', age: 35 }
];
const names = users.map(user => user.name);
console.log(names);  // ["Alice", "Bob", "Charlie"]
```

## 예시 3: `index`와 `array` 사용

`map` 메서드의 두 번째 인자인 `index`와 세 번째 인자인 `array`를 사용하는 예시입니다.

```js
const numbers = [10, 20, 30];
const indexed = numbers.map((num, index, array) => {
  return `Index: ${index}, Value: ${num}, Array length: ${array.length}`;
});
console.log(indexed);
// ["Index: 0, Value: 10, Array length: 3", "Index: 1, Value: 20, Array length: 3", "Index: 2, Value: 30, Array length: 3"]
```

## 특징

- `map` 메서드는 **원본 배열을 변경하지 않으며** 항상 새로운 배열을 반환합니다.
- `map`은 **반복 가능한 객체**에서만 사용할 수 있습니다 (예: 배열).
- **체이닝 가능**: `map` 메서드는 새로운 배열을 반환하므로 다른 배열 메서드와 체이닝하여 사용할 수 있습니다.

```js
const numbers = [1, 2, 3, 4];
const result = numbers.map(num => num * 2).filter(num => num > 5);
console.log(result);  // [6, 8]
```

## 결론

`map` 메서드는 배열의 각 항목을 변환하거나 필터링하는 데 매우 유용한 메서드입니다. 배열을 변경하지 않고, 새로운 배열을 반환하므로 불변성을 유지하면서 데이터 변환 작업을 할 수 있습니다. 다양한 예제와 함께 이 메서드를 활용하여 효율적인 코드를 작성해 보세요.
