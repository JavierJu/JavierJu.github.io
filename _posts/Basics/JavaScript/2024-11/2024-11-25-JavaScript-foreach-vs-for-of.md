---
title: "JavaScript의 `forEach`와 `for...of` 반복문 비교"
excerpt: "JavaScript에서 배열 순회를 위한 `forEach`와 `for...of` 반복문의 차이점과 활용 상황에 대해 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Array, Loop, forEach, for-of]
permalink: /JavaScript/foreach-vs-for-of/
toc: true
toc_sticky: true
date: 2024-11-25
last_modified_at: 2024-11-25
---

## JavaScript의 `forEach`와 `for...of` 반복문 비교

JavaScript에서 배열을 순회할 때 자주 사용하는 `forEach`와 `for...of` 반복문은 각각 고유한 특징과 장점을 가지고 있습니다. 이 글에서는 두 반복문의 차이점과 각 반복문을 어떤 상황에서 사용하는 것이 좋을지에 대해 자세히 알아보겠습니다.

### `forEach` 메서드

`forEach`는 배열 객체에서 사용할 수 있는 메서드로, 배열의 각 요소에 대해 주어진 콜백 함수를 실행합니다. 반복을 통해 배열을 순회하고, 각 요소에 대해 특정 작업을 수행할 때 유용합니다.

#### 사용법

```js
const numbers = [1, 2, 3, 4, 5];

numbers.forEach((num) => {
  console.log(num);
});
```

#### 특징
- `forEach`는 콜백 함수가 반환하는 값을 사용하지 않으며, 항상 `undefined`를 반환합니다.
- `break`나 `continue`와 같은 흐름 제어가 불가능합니다.
- 배열의 각 요소를 순차적으로 처리할 때 간편하게 사용됩니다.

#### 예시: 배열의 값과 인덱스를 함께 출력

```js
const numbers = [1, 2, 3, 4, 5];

numbers.forEach((num, index) => {
  console.log(`Index: ${index}, Value: ${num}`);
});
```

### `for...of` 문

`for...of`는 JavaScript의 이터러블(iterable) 객체에서 사용할 수 있는 반복문으로, 배열 외에도 문자열, `Map`, `Set` 등 다양한 이터러블 객체를 순회할 때 유용합니다.

#### 사용법

```js
const numbers = [1, 2, 3, 4, 5];

for (const num of numbers) {
  console.log(num);
}
```

#### 특징
- `for...of`는 `break`, `continue`, `return`을 사용할 수 있어, 반복문 흐름을 제어할 수 있습니다.
- 이터러블 객체의 각 요소를 순차적으로 처리합니다.
- 배열뿐만 아니라 다양한 객체에서 활용할 수 있습니다.

#### 예시: 문자열을 순회

```js
const word = "JavaScript";

for (const char of word) {
  console.log(char);
}
```

### `forEach`와 `for...of`의 차이점

| 특성                  | `forEach`                          | `for...of`                    |
|-----------------------|-------------------------------------|--------------------------------|
| **적용 대상**          | `Array` (배열 객체)                 | 이터러블 객체 전체              |
| **제어 흐름**          | `break`, `continue` 사용 불가       | 사용 가능 (`break`, `continue` 가능) |
| **인덱스 접근**         | 콜백의 두 번째 매개변수로 제공       | 별도로 처리해야 함 (`entries()` 활용) |
| **비동기 처리**         | 제한적 (콜백 내 비동기 코드는 비추천) | 비동기 루프 작성에 더 적합        |
| **가독성 및 간결성**     | 간결한 문법                       | 이터러블 작업에 더 직관적         |

### 각자의 사용 상황

#### `forEach`를 사용하는 경우
- 배열 요소를 순회하면서 작업을 수행하고, 인덱스 정보도 필요한 경우.
- 배열 변경 작업을 직접적으로 수행할 때.
- 단순 작업으로 끝나는 경우(`break`나 `continue` 없이 처리 가능).

#### `for...of`를 사용하는 경우
- 배열 외의 이터러블 객체(`Map`, `Set`, 문자열 등)를 순회해야 하는 경우.
- 순회를 도중에 멈추거나(`break`), 다음 반복으로 건너뛰고자(`continue`) 하는 경우.
- 이터러블 데이터를 더 직관적으로 다루고 싶을 때.

### `for...of`와 `entries()`를 함께 사용하기

배열에서 `for...of`를 사용하면서 인덱스를 함께 다루고 싶다면, `entries()` 메서드를 활용할 수 있습니다.

```js
const numbers = [10, 20, 30];

for (const [index, value] of numbers.entries()) {
  console.log(`Index: ${index}, Value: ${value}`);
}
```

### 결론

- **배열 요소만 간단히 순회**: `forEach`를 사용하세요.
- **이터러블 전체, 제어가 필요한 작업**: `for...of`를 사용하세요.

작업의 특성과 요구사항에 맞는 반복문을 선택하여 더 효율적으로 코드를 작성할 수 있습니다. 원하는 방식으로 순회를 제어할 수 있는 적합한 방법을 찾아보세요!
