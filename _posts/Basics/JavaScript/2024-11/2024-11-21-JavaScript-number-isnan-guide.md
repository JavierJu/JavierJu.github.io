---
title: "JavaScript에서 Number.isNaN 활용법과 예제"
excerpt: "Number.isNaN을 활용하여 숫자 유효성을 검사하는 방법과 이를 응용한 예제 코드를 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Number, Data Validation]
permalink: /JavaScript/number-isnan-guide/
toc: true
toc_sticky: true
date: 2024-11-21
last_modified_at: 2024-11-21
---

JavaScript에서 데이터를 다루는 과정에서는 값이 숫자인지 아닌지를 판별해야 하는 경우가 많습니다. 특히 사용자 입력값이나 계산 결과가 `NaN`인지 정확히 확인하는 것이 중요합니다. 이번 포스트에서는 `Number.isNaN`의 동작 방식과 이를 활용한 코드 예제를 살펴봅니다.

---

## `Number.isNaN`란?

`Number.isNaN()`는 값이 **정확히 `NaN`인지** 판별하는 메서드입니다. JavaScript에서 `NaN`은 "Not-a-Number"를 의미하며, 숫자가 아닌 값이거나 잘못된 계산 결과로 발생합니다.

### 사용법
```js
Number.isNaN(value);
```

- **`value`**: 확인하고자 하는 값.
- 반환값: `value`가 정확히 `NaN`일 경우 `true`, 그렇지 않으면 `false`.

---

## 기존 `isNaN`과의 차이점

JavaScript에는 `Number.isNaN` 외에 전역 메서드인 `isNaN`도 있습니다. 하지만 이 두 메서드의 동작 방식에는 큰 차이가 있습니다.

| 메서드       | 동작 방식                                             |
|--------------|------------------------------------------------------|
| `isNaN()`    | 값이 숫자로 변환될 수 없는 경우에도 `true` 반환        |
| `Number.isNaN()` | 값이 **정확히 `NaN`**인 경우에만 `true` 반환        |

### 예제 비교
```js
console.log(isNaN("hello"));        // true ("hello"는 숫자로 변환 불가)
console.log(Number.isNaN("hello")); // false ("hello"는 정확히 NaN이 아님)

console.log(isNaN(NaN));            // true (NaN은 숫자가 아님)
console.log(Number.isNaN(NaN));     // true (정확히 NaN)

console.log(isNaN(undefined));      // true (undefined는 숫자로 변환 불가)
console.log(Number.isNaN(undefined)); // false (undefined는 NaN이 아님)
```

---

## 실전 예제: 사용자 입력값 검증

아래 코드는 사용자가 입력한 값을 숫자로 변환한 뒤, 유효한 숫자인 경우 배열에서 해당 인덱스의 항목을 삭제합니다.

```js
const todos = ['Task 1', 'Task 2', 'Task 3'];

const index = parseInt(prompt('Ok, enter an index to delete:'));
if (!Number.isNaN(index)) {
    const deleted = todos.splice(index, 1);
    console.log(`Ok, deleted ${deleted[0]}`);
} else {
    console.log('Unknown index');
}
```

### 코드 분석

1. **사용자 입력값 처리**
    - `prompt`를 통해 입력값을 문자열로 받습니다.
    - `parseInt`를 사용하여 정수형 숫자로 변환합니다.
    - 변환 결과가 숫자가 아니면 `NaN`이 됩니다.

2. **`Number.isNaN`으로 검증**
    - `index`가 **`NaN`인지 확인**하여, 유효하지 않을 경우 오류 메시지를 출력합니다.

3. **배열 항목 삭제**
    - 유효한 숫자인 경우, `todos.splice(index, 1)`로 배열의 해당 인덱스를 삭제합니다.

---

## 실행 흐름 예제

### 1. 유효한 숫자 입력
```js
todos = ['Task 1', 'Task 2', 'Task 3'];

> 입력: 1
Ok, deleted Task 2

// todos = ['Task 1', 'Task 3']
```

### 2. 유효하지 않은 입력
```js
todos = ['Task 1', 'Task 2', 'Task 3'];

> 입력: abc
Unknown index

// todos = ['Task 1', 'Task 2', 'Task 3']
```

---

## 요약

- **`Number.isNaN`**는 값이 정확히 `NaN`인지 확인할 때 유용합니다.
- 기존 `isNaN`보다 엄격하게 동작하므로, 데이터 유효성 검사에서 더 안전하게 사용할 수 있습니다.
- 이 방법을 활용하면 사용자 입력값을 보다 효율적으로 처리할 수 있습니다.

`Number.isNaN`을 적극 활용하여 데이터 검증 로직을 안전하고 신뢰성 있게 작성해 보세요!
