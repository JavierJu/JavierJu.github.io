---
title: "JavaScript의 개요와 기초: 초보자를 위한 입문 가이드"
excerpt: "JavaScript의 기본 개념과 활용법을 소개하며, 초보자들이 알아야 할 핵심 내용을 정리합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Programming, Web Development, Beginner]
permalink: /JavaScript/intro-and-basics/
toc: true
toc_sticky: true
date: 2024-11-17
last_modified_at: 2024-11-17
---

## JavaScript 개요

JavaScript는 **웹 개발**에서 가장 널리 사용되는 프로그래밍 언어 중 하나로, **웹 페이지에 동적인 기능**을 추가하는 데 사용됩니다. 브라우저에서 실행되는 스크립트 언어로 시작했지만, 현재는 **Node.js**를 통해 서버에서도 실행할 수 있어 **프론트엔드와 백엔드 모두에서 사용**됩니다.

### 주요 특징
1. **경량 언어**:
   - 문법이 간단하고 가볍습니다.
2. **동적 타이핑**:
   - 변수의 데이터 유형을 미리 선언하지 않아도 됩니다.
3. **인터프리터 언어**:
   - 브라우저에서 코드가 바로 실행됩니다(컴파일 불필요).
4. **이벤트 기반**:
   - 클릭, 입력, 스크롤 등의 사용자 동작에 반응하는 기능 구현에 최적화되어 있습니다.
5. **다중 플랫폼**:
   - 다양한 운영 체제와 장치에서 동작합니다.

### 주요 용도
- **웹 페이지 상호작용**:
  - 버튼 클릭, 슬라이더 애니메이션, 모달 창 등.
- **서버 개발**:
  - Node.js를 통해 서버 및 API 개발.
- **모바일 및 데스크톱 앱 개발**:
  - React Native, Electron 같은 프레임워크 사용.
- **게임 개발**:
  - 웹 게임 개발을 위한 엔진(예: Phaser.js) 활용.

---

## JavaScript 기초

### 1. 변수 선언
JavaScript에서 변수를 선언하는 방법:
- `var`: 과거 방식. 블록 범위가 아닌 함수 범위.
- `let`: 블록 범위 변수 선언.
- `const`: 상수 선언(값 변경 불가능).

```js
let name = "Javier"; // 블록 범위 변수
const age = 25;      // 상수
var country = "Japan"; // 함수 범위 변수
```

---

### 2. 데이터 타입
JavaScript의 기본 데이터 타입:
- **원시 타입 (Primitive Types)**:
  - `string`, `number`, `boolean`, `undefined`, `null`, `symbol`, `bigint`.
- **객체 타입**:
  - `object`, `array`, `function` 등.
```js
let text = "Hello";       // string
let number = 42;          // number
let isComplete = true;    // boolean
let notDefined;           // undefined
let emptyValue = null;    // null
```

---

### 3. 조건문
```js
if (age > 18) {
  console.log("Adult");
} else {
  console.log("Minor");
}
```

---

### 4. 반복문
```js
for (let i = 0; i < 5; i++) {
  console.log("Count: " + i);
}

let fruits = ["Apple", "Banana", "Cherry"];
for (let fruit of fruits) {
  console.log(fruit);
}
```

---

### 5. 함수
- **선언형 함수**:
```js
function greet(name) {
  return `Hello, ${name}`;
}
console.log(greet("Javier")); // Hello, Javier
```

- **화살표 함수**:
```js
const greet = (name) => `Hello, ${name}`;
console.log(greet("Javier")); // Hello, Javier
```

---

### 6. 이벤트 처리
```js
document.getElementById("myButton").addEventListener("click", () => {
  alert("Button clicked!");
});
```

---

### 7. DOM 조작
```js
document.getElementById("myText").textContent = "Updated Text!";
```

---

### 8. JSON과 데이터 처리
```js
let jsonData = '{"name": "Javier", "age": 25}';
let user = JSON.parse(jsonData); // JSON 문자열 -> 객체 변환
console.log(user.name); // Javier
```

---

## 추천 학습 순서
1. **HTML/CSS와의 연계 이해**:
   - HTML과 CSS로 만든 요소에 JavaScript를 추가하여 동적 기능 구현.
2. **기본 문법**:
   - 변수, 조건문, 반복문, 함수.
3. **DOM 조작**:
   - 웹 페이지의 요소를 JavaScript로 읽고 수정.
4. **이벤트 처리**:
   - 사용자와의 상호작용 처리.
5. **API와 Fetch**:
   - 웹 서버에서 데이터를 가져오고 처리.
6. **모던 JavaScript**:
   - ES6+ 문법(화살표 함수, 템플릿 리터럴, 모듈 등).

작은 프로젝트로 실습하면서 경험을 쌓아보세요. 예를 들어, **To-Do List** 같은 프로젝트를 구현해 보는 것도 좋은 시작입니다! 🚀
