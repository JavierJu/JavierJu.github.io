---
title: "JavaScript 숫자와 NaN, 그리고 연산자 완벽 가이드"
excerpt: "JavaScript에서 숫자와 연산자, NaN의 작동 원리와 활용 방법에 대해 자세히 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Numbers, Operators, NaN, Infinity]
permalink: /JavaScript/numbers-and-nan-guide/
toc: true
toc_sticky: true
date: 2024-11-19
last_modified_at: 2024-11-19
---

JavaScript는 강력하고 유연한 숫자 처리를 제공합니다. 이 글에서는 숫자(`Number`) 타입, 다양한 연산자, 그리고 `NaN`과 같은 특수 값에 대해 알아보겠습니다.

## JavaScript의 숫자 (Numbers)
JavaScript에서 숫자는 모두 `Number` 타입으로 처리됩니다. 정수와 실수 구분 없이 **64비트 부동소수점 형식**으로 저장되며, 아래와 같은 형태를 가질 수 있습니다:

- **정수**: `1`, `42`, `-7`
- **소수(실수)**: `3.14`, `-0.001`, `2e3` (2 × 10³ = 2000)
- **특수 숫자 값**: `Infinity`, `-Infinity`, `NaN`

### 숫자 관련 메서드
다양한 메서드를 사용하여 숫자를 조작하거나 검증할 수 있습니다:
- `Number.isInteger(value)`: 정수 여부 확인
- `Number.isNaN(value)`: 값이 `NaN`인지 확인
- `Math.floor()`, `Math.ceil()`, `Math.round()`: 내림, 올림, 반올림 처리
- `parseInt()`, `parseFloat()`: 문자열을 숫자로 변환

---

## 연산자 (Operators)
JavaScript는 숫자를 다룰 때 다양한 연산자를 제공합니다.

### 기본 산술 연산자

| 연산자 | 설명                | 예시            | 결과 |
|--------|---------------------|-----------------|------|
| `+`    | 덧셈                | `5 + 3`         | `8`  |
| `-`    | 뺄셈                | `10 - 7`        | `3`  |
| `*`    | 곱셈                | `4 * 2`         | `8`  |
| `/`    | 나눗셈              | `9 / 3`         | `3`  |
| `%`    | 나머지              | `10 % 4`        | `2`  |
| `**`   | 거듭제곱 (ES6 지원)  | `2 ** 3`        | `8`  |

### 단항 연산자

| 연산자 | 설명          | 예시     | 결과 |
|--------|---------------|----------|------|
| `+`    | 숫자로 변환   | `+"3"`   | `3`  |
| `-`    | 부호 반전     | `-5`     | `-5` |

### 증감 연산자

| 연산자 | 설명           | 예시            | 결과   |
|--------|----------------|-----------------|--------|
| `++`   | 1 증가 (전위/후위) | `x++`, `++x`   | 값 증가 |
| `--`   | 1 감소 (전위/후위) | `x--`, `--x`   | 값 감소 |

---

## NaN (Not-a-Number)
`NaN`은 "숫자가 아님"을 나타내는 값으로, 잘못된 수학 연산이 수행될 때 반환됩니다.

### `NaN`이 발생하는 경우
- **부적절한 변환**:
  ```js
  Number("abc"); // NaN
  ```
- **잘못된 수학 연산**:
  ```js
  Math.sqrt(-1); // NaN
  0 / 0;         // NaN
  ```

### `NaN`의 특징
- `NaN`은 자기 자신과도 같지 않습니다:
  ```js
  console.log(NaN === NaN); // false
  ```
- `isNaN(value)` 또는 `Number.isNaN(value)`를 사용하여 확인할 수 있습니다:
  ```js
  console.log(isNaN("hello"));         // true
  console.log(Number.isNaN("hello")); // false
  console.log(Number.isNaN(NaN));     // true
  ```

---

## Infinity와 -Infinity
JavaScript는 무한대를 나타내는 두 가지 특수 숫자 값을 제공합니다:
- **`Infinity`**: 값이 양의 무한대일 때 사용.
  ```js
  console.log(1 / 0); // Infinity
  ```
- **`-Infinity`**: 값이 음의 무한대일 때 사용.
  ```js
  console.log(-1 / 0); // -Infinity
  ```

---

## 숫자 변환
JavaScript에서 값을 숫자로 변환하는 방법은 다음과 같습니다:

### 명시적 변환
- `Number(value)`: 숫자로 변환
  ```js
  Number("123"); // 123
  Number("abc"); // NaN
  ```
- `parseInt(value)`, `parseFloat(value)`: 문자열을 숫자로 변환
  ```js
  parseInt("42px");  // 42
  parseFloat("3.14"); // 3.14
  ```

### 암시적 변환
JavaScript는 연산에 따라 값을 자동으로 숫자로 변환합니다:
  ```js
  console.log("5" - 2);  // 3 ("5"가 숫자로 변환)
  console.log("5" + 2);  // "52" (문자열 연결)
  ```

---

JavaScript의 숫자와 관련된 모든 기본 개념을 익히셨나요? 위 내용을 실습하며 정확히 이해해보세요! 😊
