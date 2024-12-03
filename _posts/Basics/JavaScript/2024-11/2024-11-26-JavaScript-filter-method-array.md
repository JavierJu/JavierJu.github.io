---
title: "JavaScript filter 메서드: 배열에서 조건에 맞는 요소 추출하기"
excerpt: "JavaScript의 filter 메서드를 사용하여 배열에서 조건을 만족하는 요소를 추출하는 방법과 활용 예제를 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Array, filter, Function]
permalink: /JavaScript/filter-method-array/
toc: true
toc_sticky: true
date: 2024-11-26
last_modified_at: 2024-11-26
---

JavaScript에서 `filter` 메서드는 배열에서 조건에 맞는 요소들을 추출하는 데 유용한 방법입니다. 이 메서드는 기존 배열을 변경하지 않고, 주어진 조건을 만족하는 **새로운 배열**을 반환합니다. 아래에서 `filter` 메서드의 사용법과 다양한 활용 예제를 소개합니다.

### `filter` 메서드 기본 사용법

`filter`는 배열의 각 요소에 대해 주어진 **callback 함수**를 실행하고, 그 결과가 `true`인 요소들만 모아서 새로운 배열을 반환합니다.

#### 문법:
```js
const newArray = array.filter(callback(element[, index[, array]])[, thisArg]);
```

- **callback**: 배열의 각 요소에 대해 실행되는 함수입니다. 이 함수는 각 요소에 대해 `true` 또는 `false`를 반환하며, `true`일 경우 해당 요소가 새로운 배열에 포함됩니다.
- **thisArg** (선택): `callback` 함수 내에서 사용될 `this` 값을 설정할 수 있습니다.

### 기본 사용 예제

#### 짝수만 필터링
```js
const numbers = [1, 2, 3, 4, 5];

// 짝수만 필터링
const evenNumbers = numbers.filter(num => num % 2 === 0);

console.log(evenNumbers); // [2, 4]
```

### 객체 배열 필터링

배열의 요소가 객체일 때도 `filter`를 유용하게 사용할 수 있습니다. 예를 들어, 나이가 20세 이상인 사용자를 추출할 수 있습니다.

```js
const users = [
  { id: 1, name: 'Alice', age: 25 },
  { id: 2, name: 'Bob', age: 18 },
  { id: 3, name: 'Charlie', age: 30 }
];

// 20세 이상인 사용자 필터링
const adults = users.filter(user => user.age >= 20);

console.log(adults);
/*
[
  { id: 1, name: 'Alice', age: 25 },
  { id: 3, name: 'Charlie', age: 30 }
]
*/
```

### 인덱스를 이용한 필터링

`filter` 메서드에서 두 번째 매개변수로 인덱스를 사용할 수 있습니다. 이를 통해 특정 인덱스의 요소를 필터링할 수 있습니다.

```js
const numbers = [10, 20, 30, 40, 50];

// 인덱스가 짝수인 요소만 필터링
const evenIndexNumbers = numbers.filter((num, index) => index % 2 === 0);

console.log(evenIndexNumbers); // [10, 30, 50]
```

### 조건을 만족하지 않으면 빈 배열 반환

`filter`는 조건을 만족하는 요소가 하나도 없다면 빈 배열을 반환합니다.

```js
const numbers = [1, 3, 5, 7];

// 10보다 큰 숫자 필터링
const greaterThanTen = numbers.filter(num => num > 10);

console.log(greaterThanTen); // []
```

### 응용 예제

#### 1. **중복 요소 제거**

배열에서 중복된 값을 제거하는 데 `filter`를 활용할 수 있습니다.

```js
const numbers = [1, 2, 2, 3, 4, 4, 5];
const uniqueNumbers = numbers.filter((num, index, arr) => arr.indexOf(num) === index);

console.log(uniqueNumbers); // [1, 2, 3, 4, 5]
```

#### 2. **Falsy 값 제거**

배열에서 `null`, `undefined`, `false`, `0` 등의 Falsy 값을 제거할 수 있습니다.

```js
const mixedArray = [0, 1, false, 2, '', 3, null, undefined, NaN];
const truthyArray = mixedArray.filter(Boolean);

console.log(truthyArray); // [1, 2, 3]
```

### 결론

JavaScript의 `filter` 메서드는 배열에서 조건에 맞는 요소를 추출하는 데 매우 유용하며, 코드의 가독성을 높이는 데 도움이 됩니다. 다양한 상황에서 `filter`를 활용하여 데이터를 처리하고, 원하는 값을 효율적으로 얻을 수 있습니다.
