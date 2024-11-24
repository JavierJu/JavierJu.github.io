---
title: "JavaScript에서의 판단내리기: 조건문과 논리 연산의 모든 것"
excerpt: "JavaScript에서 조건문과 논리 연산을 활용한 판단내리기 방법을 비교 연산자, truthy/falsy 값, 삼항 연산자 등과 함께 자세히 살펴봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, 조건문, 논리 연산자, 프로그래밍 기초]
permalink: /JavaScript/decision-making/
toc: true
toc_sticky: true
date: 2024-11-18
last_modified_at: 2024-11-18
---

## JavaScript에서의 판단내리기란?

JavaScript는 프로그램이 특정 조건에 따라 서로 다른 작업을 수행할 수 있도록 다양한 조건문과 연산자를 제공합니다. 여기서는 비교 연산자, truthy/falsy 값, 조건문(if, else if, else), 삼항 연산자, 논리 연산자, switch문 등 판단내리기의 주요 요소를 자세히 설명합니다.

---

### 1. 비교 연산자
비교 연산자는 두 값을 비교하여 `true` 또는 `false`를 반환합니다.

| 연산자  | 설명                          | 예시 (`a = 5, b = "5", c = 10`) |
|---------|-------------------------------|---------------------------------|
| `==`    | 값이 같으면 `true`            | `a == b` → `true`              |
| `===`   | 값과 자료형이 같으면 `true`   | `a === b` → `false`            |
| `!=`    | 값이 다르면 `true`            | `a != c` → `true`              |
| `!==`   | 값 또는 자료형이 다르면 `true`| `a !== b` → `true`             |
| `<`     | 작은 경우 `true`              | `a < c` → `true`               |
| `<=`    | 작거나 같은 경우 `true`       | `a <= b` → `true`              |
| `>`     | 큰 경우 `true`                | `c > a` → `true`               |
| `>=`    | 크거나 같은 경우 `true`       | `a >= c` → `false`             |

---

### 2. 이중 등호(`==`)와 삼중 등호(`===`)
- **이중 등호 (`==`)**: 값만 비교하며, 자료형을 **자동 변환**합니다.
- **삼중 등호 (`===`)**: 값과 자료형 모두를 비교하며, 자료형 변환 없이 정확히 비교합니다.

```js
const a = 5;
const b = "5";

console.log(a == b);  // true
console.log(a === b); // false
```

> **팁**: 자료형까지 명확히 비교해야 하는 경우 `===` 사용을 권장합니다.

---

### 3. 조건문 (if, else if, else)

#### 기본 구조
```js
if (조건1) {
  // 조건1이 true일 때 실행
} else if (조건2) {
  // 조건2가 true일 때 실행
} else {
  // 위 조건들이 모두 false일 때 실행
}
```

#### 예시
```js
const score = 85;

if (score >= 90) {
  console.log("A 학점");
} else if (score >= 80) {
  console.log("B 학점");
} else {
  console.log("C 학점");
}
```

#### 조건부 네스팅
조건문 안에 또 다른 조건문을 중첩하여 사용 가능합니다.
```js
const temperature = 35;

if (temperature > 30) {
  if (temperature > 40) {
    console.log("너무 더움");
  } else {
    console.log("더움");
  }
} else {
  console.log("적당함");
}
```

---

### 4. 논리 연산자 (AND, OR, NOT)
#### **AND (`&&`)**
모든 조건이 `true`일 때만 `true`를 반환합니다.
```js
const age = 25;
const hasID = true;

if (age >= 18 && hasID) {
  console.log("입장 가능");
}
```

#### **OR (`||`)**
하나라도 `true`면 `true`를 반환합니다.
```js
const isWeekend = false;
const isHoliday = true;

if (isWeekend || isHoliday) {
  console.log("휴일입니다");
}
```

#### **NOT (`!`)**
조건을 반대로 변환합니다.
```js
const loggedIn = false;

if (!loggedIn) {
  console.log("로그인하세요");
}
```

---

### 5. Truthy와 Falsy 값
JavaScript는 조건 평가 시, 특정 값을 **truthy**(참처럼 간주) 또는 **falsy**(거짓처럼 간주)로 해석합니다.

#### **Falsy 값**
- `false`
- `0`
- `""` (빈 문자열)
- `null`
- `undefined`
- `NaN`

#### **Truthy 값**
Falsy 값이 아닌 모든 값은 truthy로 간주됩니다.  
예시:
```js
if ("hello") {
  console.log("Truthy"); // 실행됨
}

if (0) {
  console.log("Falsy"); // 실행되지 않음
}
```

---

### 6. 조건부 연산자 (삼항 연산자)
삼항 연산자를 사용하면 단순한 조건문을 한 줄로 작성할 수 있습니다.

```js
const age = 20;
const message = age >= 18 ? "성인" : "미성년자";
console.log(message); // "성인"
```

---

### 7. switch 조건문
**if-else**의 대안으로, 특정 값과 여러 값을 비교할 때 사용합니다.

```js
const day = "월요일";

switch (day) {
  case "월요일":
    console.log("한 주의 시작입니다!");
    break;
  case "금요일":
    console.log("주말이 곧 옵니다!");
    break;
  default:
    console.log("평범한 날입니다.");
}
```

---

### 8. 종합 예제
모든 요소를 활용한 프로그램 예제입니다.

```js
const userInput = "yes";
const age = 17;

// truthy/falsy 값 확인
if (userInput) {
  console.log("사용자가 입력했습니다");
}

// AND, OR 조건 및 중첩 if 사용
if (age >= 18 && userInput === "yes") {
  console.log("성인 인증 완료");
} else if (age < 18 || userInput === "no") {
  console.log("미성년자 인증");
}

// switch와 삼항 연산자
const accessLevel = age >= 18 ? "성인" : "미성년자";

switch (accessLevel) {
  case "성인":
    console.log("모든 권한을 가집니다");
    break;
  case "미성년자":
    console.log("제한된 권한만 있습니다");
    break;
  default:
    console.log("알 수 없는 사용자");
}
```
