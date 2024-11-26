---
title: "JavaScript의 Boolean: Truthy와 Falsy를 이해하기"
excerpt: "JavaScript에서 boolean의 작동 원리와 truthy, falsy 값을 이해하는 방법을 코드 예제와 함께 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Boolean, Data Types, Truthy, Falsy]
permalink: /JavaScript/boolean-truthy-falsy/
toc: true
toc_sticky: true
date: 2024-11-23
last_modified_at: 2024-11-23
---

## JavaScript의 Boolean 개념 이해하기

JavaScript에서 `boolean`은 논리적 참(`true`) 또는 거짓(`false`)을 나타내는 데이터 타입입니다. 이는 조건문, 비교, 논리 연산 등에서 자주 사용됩니다. 이 글에서는 boolean 값의 작동 원리와 함께 truthy 및 falsy 값을 코드 예제와 함께 살펴보겠습니다.

---

### Boolean 값을 반환하는 주요 상황

#### 1. 조건문
`if (조건)` 구문에서 조건이 `true`로 평가되면 코드 블록이 실행됩니다.

```js
if (true) {
  console.log("이 메시지는 항상 출력됩니다."); // true인 경우 실행
}
```

#### 2. 비교 연산
비교 연산자는 항상 `boolean`을 반환합니다.

```js
console.log(5 > 3);    // true
console.log(5 === '5'); // false (엄격한 비교)
console.log(10 !== 5);  // true
```

#### 3. 논리 연산
논리 연산자(`&&`, `||`, `!`)를 사용하면 `boolean` 값이나 평가된 값을 반환합니다.

```js
console.log(true && false); // false
console.log(false || true); // true
console.log(!true);         // false
```

---

### Falsy와 Truthy 값

JavaScript에서는 `boolean` 컨텍스트에서 참(`truthy`) 또는 거짓(`falsy`)으로 간주되는 값이 있습니다.

#### Falsy 값
아래 값들은 `false`로 평가됩니다:
- `false`
- `0` (숫자 0)
- `-0` (음의 0)
- `""` (빈 문자열)
- `null`
- `undefined`
- `NaN` (Not a Number)

예제:
```js
if (0) {
  console.log("출력되지 않습니다."); // 0은 falsy
}

if ("") {
  console.log("이 메시지도 출력되지 않습니다."); // 빈 문자열은 falsy
}
```

#### Truthy 값
Falsy가 아닌 모든 값은 `true`로 평가됩니다.  
Truthy 값의 예는 다음과 같습니다:
- 숫자(0이 아닌 모든 값)
- 문자열(길이가 0이 아닌 모든 문자열)
- 객체, 배열
- 심지어 빈 배열(`[]`)이나 빈 객체(`{}`)도 truthy입니다.

예제:
```js
if (1) {
  console.log("출력됩니다."); // 1은 truthy
}

if ("hello") {
  console.log("이 문자열도 truthy입니다.");
}

if ({}) {
  console.log("빈 객체도 truthy입니다.");
}
```

---

### Boolean() 함수

`Boolean(value)` 함수는 주어진 값을 명시적으로 `true` 또는 `false`로 변환합니다.

#### Truthy → True 변환 예시:
```js
console.log(Boolean(42));          // true
console.log(Boolean("JavaScript")); // true
console.log(Boolean([]));           // true
console.log(Boolean({}));           // true
```

#### Falsy → False 변환 예시:
```js
console.log(Boolean(0));        // false
console.log(Boolean(""));       // false
console.log(Boolean(null));     // false
console.log(Boolean(undefined));// false
console.log(Boolean(NaN));      // false
```

---

### Number() 함수

`Number(value)`는 값을 숫자로 변환하며, boolean 값도 숫자로 변환됩니다.

#### 변환 결과:

| 값         | 결과 |
|------------|------|
| `true`     | `1`  |
| `false`    | `0`  |
| `null`     | `0`  |
| `undefined`| `NaN`|
| `""`       | `0`  |
| `"123"`    | `123`|
| `"abc"`    | `NaN`|

| 값         | 결과 |
|------------|------|
| `true`     | `1`  |
| `false`    | `0`  |
| `null`     | `0`  |
| `undefined`| `NaN`|
| `""`       | `0`  |
| `"123"`    | `123`|
| `"abc"`    | `NaN`|

예제:
```js
console.log(Number(true));     // 1
console.log(Number(false));    // 0
console.log(Number("42"));     // 42
console.log(Number(""));       // 0
console.log(Number(undefined));// NaN
```

---

### 자주 헷갈리는 부분

#### 빈 배열과 빈 객체
- `[]`와 `{}`는 truthy이지만, 특정 상황에서 예상치 못한 결과를 초래할 수 있습니다.

예제:
```js
console.log(Boolean([])); // true
console.log(Boolean({})); // true

console.log([] == false); // true (빈 배열은 비교 시 falsy로 평가)
console.log({} == false); // false (객체는 falsy가 아님)
```

#### `undefined`와 `null`
- `undefined`와 `null` 모두 falsy이지만, 서로 같지 않습니다(`==` 비교는 true).

예제:
```js
console.log(undefined == null); // true
console.log(undefined === null); // false
```

---

### 활용 예시

#### 조건문 간소화
`Boolean` 값을 확인하려면 명시적 비교 없이 바로 사용할 수 있습니다.

```js
let isLoggedIn = false;

if (isLoggedIn) {
  console.log("로그인 상태입니다.");
} else {
  console.log("로그아웃 상태입니다.");
}
```

#### 짧은 논리 평가
`||`와 `&&`를 활용하여 값을 간단히 설정하거나 조건을 평가합니다.

```js
let name = null;
console.log(name || "익명 사용자"); // "익명 사용자" (null은 falsy)

let isActive = true;
console.log(isActive && "활성화됨"); // "활성화됨" (true는 truthy)
```

---

JavaScript의 `boolean`은 단순해 보이지만, truthy와 falsy 값의 특성을 잘 이해하면 코드 작성이 더 쉬워집니다. 다양한 값을 직접 `Boolean()` 함수로 테스트하며 익숙해져 보세요!
