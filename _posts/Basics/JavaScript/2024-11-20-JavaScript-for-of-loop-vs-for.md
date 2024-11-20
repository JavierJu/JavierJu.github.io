---
title: "JavaScript의 for...of 루프와 for 루프의 차이점과 활용법"
excerpt: "JavaScript에서 배열 및 iterable 객체를 효율적으로 순회할 수 있는 for...of 루프의 사용법과 for 루프와의 차이점을 예제와 함께 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, ES6, Loops, Programming]
permalink: /JavaScript/for-of-loop-vs-for/
toc: true
toc_sticky: true
date: 2024-11-20
last_modified_at: 2024-11-20
---

## for 루프와 for...of 루프 비교

JavaScript에서 배열이나 iterable 객체를 순회할 때 `for...of` 루프는 구문이 간단하고 가독성이 뛰어납니다. 기존의 `for` 루프와는 사용 목적과 동작 방식에서 차이가 있습니다.

### 1. 주요 차이점

| **특징**                | **`for` 루프**                                                         | **`for...of` 루프**                                                 |
|--------------------------|------------------------------------------------------------------------|----------------------------------------------------------------------|
| **사용 대상**            | 숫자 기반 인덱스 접근 가능 객체 (보통 배열)                            | 모든 iterable 객체 (배열, 문자열, Map, Set 등)                      |
| **반복 요소**            | 인덱스 (`i`)를 통해 값에 접근                                          | 직접 값(value)을 반환                                               |
| **구문 복잡도**          | 인덱스 초기화, 조건, 증감식 필요                                       | 간단한 구문                                                         |
| **범위**                | 필요 시 인덱스를 기반으로 특정 범위 순회 가능                           | 전체 iterable 객체 순회 (부분 순회 시 추가 조건 필요)               |
| **사용 편의성**          | 복잡한 작업에서 유리 (인덱스 제어, 중첩된 배열 등)                     | 값을 단순히 순회할 때 더 직관적                                      |

---

### 2. 예제 코드로 비교

#### (1) `for` 루프를 사용한 배열 순회
```js
const arr = [10, 20, 30];

for (let i = 0; i < arr.length; i++) {
  console.log(`Index ${i}: Value ${arr[i]}`);
}
```

#### 출력
```
Index 0: Value 10
Index 1: Value 20
Index 2: Value 30
```

#### (2) `for...of` 루프를 사용한 배열 순회
```js
const arr = [10, 20, 30];

for (const value of arr) {
  console.log(`Value: ${value}`);
}
```

#### 출력
```
Value: 10
Value: 20
Value: 30
```

---

### 3. 문자열 순회

#### `for...of`를 사용한 문자열 순회
```js
const str = "hello";

for (const char of str) {
  console.log(`Character: ${char}`);
}
```

#### 출력
```
Character: h
Character: e
Character: l
Character: l
Character: o
```

---

### 4. Map과 Set에서의 활용

#### (1) `Map` 순회
```js
const map = new Map([
  ["name", "Alice"],
  ["age", 25],
]);

for (const [key, value] of map) {
  console.log(`${key}: ${value}`);
}
```

#### 출력
```
name: Alice
age: 25
```

#### (2) `Set` 순회
```js
const set = new Set([1, 2, 3]);

for (const value of set) {
  console.log(value);
}
```

#### 출력
```
1
2
3
```

---

### 5. 주의: 객체(Object)는 `for...of`로 순회할 수 없음

객체는 iterable이 아니므로 `for...of`로 직접 순회할 수 없습니다. 대신 `Object.keys()`, `Object.values()`, `Object.entries()`와 함께 사용해야 합니다.

#### 예제: 객체의 키와 값을 순회하기
```js
const obj = { a: 1, b: 2, c: 3 };

for (const [key, value] of Object.entries(obj)) {
  console.log(`${key}: ${value}`);
}
```

#### 출력
```
a: 1
b: 2
c: 3
```

---

### 6. 요약
- **`for` 루프**: 인덱스가 필요한 작업에 적합.
- **`for...of` 루프**: 값만 필요하고, iterable 객체를 순회할 때 간단하고 직관적.

`for...of` 루프는 iterable 객체를 다룰 때 코드를 간결하고 이해하기 쉽게 만들어 줍니다. 상황에 따라 적절한 루프를 선택해 사용하세요!

