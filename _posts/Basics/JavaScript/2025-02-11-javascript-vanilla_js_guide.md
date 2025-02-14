---
title: "Vanilla JS: 순수 JavaScript로 웹 개발하기"
excerpt: "Vanilla JS는 외부 라이브러리 없이 순수 JavaScript로 개발하는 방식입니다. 이 블로그 포스트에서는 Vanilla JS의 특징, 주요 개념, 예제 코드 등을 자세히 설명합니다."
categories:
  - JavaScript
  - Frontend
tags:
  - [JavaScript, Vanilla JS, 웹 개발, 프론트엔드]
permalink: /javascript/vanilla-js/
toc: true
toc_sticky: true
date: 2025-02-11
last_modified_at: 2025-02-11
---

## Vanilla JS란?

**Vanilla JS**는 별도의 라이브러리나 프레임워크 없이 **순수한 JavaScript**만으로 개발하는 방식을 의미합니다. React, Vue, jQuery 같은 추가적인 도구 없이 **기본 JavaScript의 기능만 활용**하는 방식입니다. 

Vanilla JS는 JavaScript의 본질을 이해하는 데 중요하고, 가벼운 프로젝트에서는 굳이 라이브러리를 도입할 필요 없이 효율적인 선택이 될 수 있습니다.

---

## 1️⃣ Vanilla JS의 특징

### ✅ 가볍다 (Lightweight)
- 추가 라이브러리 없이 JavaScript 엔진만 사용하기 때문에 **빠르고 가벼움**  
- 외부 라이브러리 로딩 시간이 필요 없으므로 성능이 최적화됨  

### ✅ 브라우저 친화적 (Browser-Compatible)
- HTML, CSS와 함께 **기본적으로 모든 브라우저에서 실행 가능**  
- jQuery 같은 라이브러리가 필요했던 기능들도 최신 JS (ES6+)에서 대부분 해결됨  

### ✅ 학습의 중요성
- Vanilla JS를 잘 이해하면 다른 라이브러리나 프레임워크를 배우는 데도 큰 도움이 됩니다.  
- 프레임워크 없이도 JavaScript의 **핵심 개념(이벤트, DOM 조작, 비동기 처리 등)**을 익힐 수 있습니다.  

---

## 2️⃣ Vanilla JS 주요 개념

### 🎯 1. DOM 조작
Vanilla JS를 사용하면 jQuery 없이도 쉽게 DOM을 조작할 수 있습니다.

#### 📌 요소 선택 (Query Selectors)
```js
const element = document.querySelector("#myElement");  // ID 선택
const elements = document.querySelectorAll(".myClass"); // 클래스 선택 (여러 개)
```

#### 📌 HTML 요소 변경
```js
const element = document.getElementById("demo");
element.innerHTML = "Hello, Vanilla JS!";
```

#### 📌 CSS 스타일 변경
```js
element.style.color = "blue";
element.style.fontSize = "20px";
```

#### 📌 클래스 추가/제거
```js
element.classList.add("active");
element.classList.remove("hidden");
element.classList.toggle("dark-mode");
```

---

### 🎯 2. 이벤트 핸들링
```js
document.getElementById("btn").addEventListener("click", function () {
    alert("버튼이 클릭되었습니다!");
});
```

- `addEventListener`를 사용하면 다양한 이벤트(`click`, `mouseover`, `keydown` 등)를 감지할 수 있습니다.  
- jQuery에서 `$("#btn").click(function() {...})`와 같은 기능을 순수 JS로 구현하는 방식입니다.

---

### 🎯 3. 비동기 처리 (Fetch API)
Vanilla JS에서도 Ajax 요청을 쉽게 보낼 수 있습니다.

#### 📌 Fetch API 사용 예제
```js
fetch("https://jsonplaceholder.typicode.com/posts/1")
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error("에러 발생:", error));
```
- `fetch()`를 사용하면 서버에서 데이터를 가져올 수 있습니다.  
- `.then()`과 `.catch()`를 활용해 **비동기 처리 (Async/Await도 가능)**  

#### 📌 Async/Await 적용
```js
async function fetchData() {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error("에러 발생:", error);
    }
}
fetchData();
```

---

### 🎯 4. 로컬 스토리지 (LocalStorage)
- 브라우저에 데이터를 저장하고 유지하는 기능  

```js
localStorage.setItem("username", "Javier");
const user = localStorage.getItem("username");
console.log(user); // "Javier"

localStorage.removeItem("username"); // 데이터 삭제
```

---

### 🎯 5. 모듈 시스템 (ES6 Modules)
크기가 큰 프로젝트에서는 JavaScript 코드를 모듈화할 필요가 있습니다.

#### 📌 파일 분리 (module.js)
```js
export function greet(name) {
    return `Hello, ${name}!`;
}
```

#### 📌 다른 파일에서 가져오기 (main.js)
```js
import { greet } from "./module.js";

console.log(greet("Javier")); // "Hello, Javier!"
```

---

## 3️⃣ Vanilla JS vs 프레임워크 비교

| 비교 항목 | Vanilla JS | React/Vue 등 프레임워크 |
|-----------|------------|----------------------|
| 성능 | 가볍고 빠름 | 프레임워크 자체 오버헤드 있음 |
| 학습 난이도 | 쉬움 (기본적인 JS 문법만 이해하면 가능) | 높음 (추가 개념 필요: 상태 관리, 컴포넌트 등) |
| 확장성 | 프로젝트가 커지면 관리 어려움 | 대규모 프로젝트에 적합 |
| 코드 유지보수 | 구조화된 패턴 없음 | 컴포넌트 기반으로 유지보수 용이 |
| 상태 관리 | 직접 구현 필요 (예: `localStorage`) | Redux, Vuex 등 사용 가능 |

---

## 4️⃣ Vanilla JS를 언제 사용할까?

✅ **작고 간단한 프로젝트**  
✅ **라이브러리 의존성을 줄이고 싶을 때**  
✅ **빠르고 가벼운 웹페이지가 필요할 때**  
✅ **JavaScript 기본기를 익히고 싶을 때**  

반면, **복잡한 상태 관리가 필요한 경우**나 **UI 컴포넌트가 많다면** React 같은 프레임워크를 고려하는 게 좋겠습니다.  

---

## 5️⃣ 결론
Vanilla JS는 **프레임워크 없이 JavaScript 본연의 기능을 활용하는 방법**입니다. 기본기를 탄탄히 다지면, React, Vue 같은 프레임워크를 배울 때도 더 쉽게 적응할 수 있습니다! 🚀

📌 **추천 학습 자료:**  
1. [MDN JavaScript Guide](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide)  
2. [JavaScript.info](https://javascript.info/)  

---
