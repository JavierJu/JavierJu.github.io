---
title: "JavaScript reduce 메서드의 이해와 활용"
excerpt: "JavaScript의 `reduce` 메서드를 사용하여 배열의 값을 하나로 줄이는 방법과 다양한 활용 사례를 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Array, reduce, High Order Functions]
permalink: /JavaScript/understanding-reduce-method/
toc: true
toc_sticky: true
date: 2024-11-26
last_modified_at: 2024-11-26
---

## `reduce` 메서드란?

`reduce`는 JavaScript 배열 메서드 중 하나로, 배열의 모든 요소를 하나의 값으로 줄이는 데 사용됩니다. 이 메서드는 누적값을 계산하는 데 유용하며, 다양한 상황에서 활용할 수 있습니다. `reduce`는 **배열의 각 요소를 순차적으로 처리**하고, 그 값을 **누적해서 반환**하는 방식으로 동작합니다.

### 기본 문법

```js
arr.reduce(callback, initialValue)
```

- `callback`: 배열의 각 요소에 대해 호출되는 함수.
- `initialValue`: 초기 누적값. 이 값이 없으면 배열의 첫 번째 요소가 초기값으로 사용됩니다.

### `callback` 함수의 매개변수

```js
callback(accumulator, currentValue, index, array)
```

- `accumulator`: 이전 호출에서 반환된 누적값.
- `currentValue`: 현재 처리 중인 배열의 요소.
- `index`: 현재 처리 중인 요소의 인덱스 (선택 사항).
- `array`: `reduce`를 호출한 배열 자체 (선택 사항).

---

### `reduce` 메서드 사용 예시

#### 1. 배열 합산

```js
const numbers = [1, 2, 3, 4];
const sum = numbers.reduce((acc, curr) => acc + curr, 0);

console.log(sum); // 출력: 10
```

- 초기값 `0`을 설정하여 배열의 모든 숫자를 더합니다.

---

#### 2. 배열에서 최댓값 찾기

```js
const numbers = [1, 3, 7, 2, 5];
const max = numbers.reduce((acc, curr) => (curr > acc ? curr : acc));

console.log(max); // 출력: 7
```

- 배열의 최댓값을 찾기 위한 방법으로 `reduce`를 활용합니다.

---

#### 3. 객체 배열에서 특정 값의 합계

```js
const items = [
  { name: 'apple', price: 100 },
  { name: 'banana', price: 200 },
  { name: 'cherry', price: 150 },
];

const totalPrice = items.reduce((acc, item) => acc + item.price, 0);

console.log(totalPrice); // 출력: 450
```

- 객체 배열에서 `price` 속성의 합계를 계산합니다.

---

#### 4. 중첩 배열 평탄화

```js
const nested = [[1, 2], [3, 4], [5]];

const flat = nested.reduce((acc, curr) => acc.concat(curr), []);

console.log(flat); // 출력: [1, 2, 3, 4, 5]
```

- 중첩된 배열을 평탄화하는 방법으로 `reduce`를 사용할 수 있습니다.

---

### `reduce` 메서드를 사용할 때 주의할 점

1. **초기값을 설정하지 않으면**:
   - 빈 배열에서 `reduce`를 호출하면 에러가 발생할 수 있습니다. 
   ```  js
   [].reduce((acc, curr) => acc + curr); // TypeError: Reduce of empty array with no initial value
   ```

2. **원본 배열을 변경하지 않음**:
   - `reduce`는 배열을 변형하지 않고 새로운 값을 반환합니다.

---

### `reduce`의 활용 분야

- **배열 합계, 평균, 최댓값/최솟값** 계산
- **객체 속성 그룹화** 또는 **빈도수 계산**
- **중첩 배열 평탄화**
- **상태 관리** (예: Redux에서 상태 업데이트)

`reduce`는 배열을 하나의 값으로 변환하는 매우 유용한 메서드입니다. 다양한 상황에서 활용할 수 있는 `reduce`의 강력함을 경험해 보세요!
