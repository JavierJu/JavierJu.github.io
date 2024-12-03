---
title: "JavaScript addEventListener 완벽 가이드"
excerpt: "addEventListener를 활용하여 DOM 요소에 이벤트 리스너를 추가하는 방법과 다양한 예시를 소개합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, DOM, Event Handling]
permalink: /JavaScript/addEventListener-guide/
toc: true
toc_sticky: true
date: 2024-12-1
last_modified_at: 2024-12-1
---

## `addEventListener`란?

`addEventListener`는 JavaScript에서 DOM 요소에 이벤트 리스너를 추가하는 메서드입니다. 이를 사용하면 특정 이벤트가 발생했을 때 실행될 동작(콜백 함수)을 설정할 수 있습니다.

## 구문
``` js
element.addEventListener(event, function, useCapture);
```

### 매개변수 설명
- **element**: 이벤트를 감지할 DOM 요소입니다.
- **event**: 감지할 이벤트의 종류(예: `"click"`, `"keydown"` 등).
- **function**: 이벤트가 발생했을 때 실행할 콜백 함수.
- **useCapture**: (선택적) 이벤트 캡처링 단계에서 이벤트를 처리할지 여부. 기본값은 `false`로 버블링 단계에서 이벤트가 처리됩니다.

---

## 예제 코드

### 1. 버튼 클릭 이벤트
``` html
<button id="myButton">Click Me!</button>
```

``` js
const button = document.getElementById("myButton");

button.addEventListener("click", function() {
  alert("Button clicked!");
});
```
위 코드는 버튼을 클릭했을 때 알림창이 나타나도록 설정합니다.

---

### 2. 외부 함수 사용
``` html
<button id="myButton">Click Me!</button>
```

``` js
function handleClick() {
  alert("Button clicked!");
}

const button = document.getElementById("myButton");

button.addEventListener("click", handleClick);
```
`addEventListener`에 외부 함수를 전달하여 코드 재사용성을 높일 수 있습니다.

---

### 3. 여러 이벤트 리스너 추가
``` js
const button = document.getElementById("myButton");

button.addEventListener("click", function() {
  alert("First click handler");
});

button.addEventListener("click", function() {
  alert("Second click handler");
});
```
위 코드는 동일한 요소에 여러 이벤트 리스너를 추가하여 순차적으로 실행되게 합니다.

---

### 4. 이벤트 캡처링과 버블링
``` html
<div id="parent">
  <button id="child">Click Me!</button>
</div>
```

``` js
const parent = document.getElementById("parent");
const child = document.getElementById("child");

parent.addEventListener("click", function() {
  alert("Parent clicked!");
}, true); // 캡처링 단계에서 실행

child.addEventListener("click", function() {
  alert("Child clicked!");
});
```
- 캡처링 단계에서는 부모의 이벤트가 먼저 실행됩니다.
- 기본 버블링 동작과 비교할 때 유연한 이벤트 흐름 제어가 가능합니다.

---

### 5. 이벤트 리스너 제거
``` html
<button id="myButton">Click Me!</button>
```

``` js
function handleClick() {
  alert("Button clicked!");
}

const button = document.getElementById("myButton");

button.addEventListener("click", handleClick);

// 3초 후 이벤트 리스너 제거
setTimeout(() => {
  button.removeEventListener("click", handleClick);
  alert("Event listener removed!");
}, 3000);
```
`removeEventListener`를 사용하면 더 이상 특정 이벤트를 감지하지 않도록 설정할 수 있습니다.

---

### 6. 한 번만 실행되는 이벤트 (`once` 옵션)
``` html
<button id="myButton">Click Me!</button>
```

``` js
const button = document.getElementById("myButton");

button.addEventListener("click", function() {
  alert("Button clicked!");
}, { once: true });
```
이 코드는 이벤트가 한 번 발생한 후 자동으로 리스너를 제거합니다.

---

## 정리

`addEventListener`는 DOM 요소에 이벤트를 유연하게 추가하고 제어할 수 있는 강력한 메서드입니다. 캡처링, 버블링, 이벤트 리스너 제거 등 다양한 기능을 활용하여 동적인 사용자 인터페이스를 구현할 수 있습니다.

> 이벤트 처리와 관련된 더 많은 내용을 알고 싶다면 댓글로 질문해주세요! 😊
