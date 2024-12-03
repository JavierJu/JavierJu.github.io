---
title: "JavaScript DOM 이벤트에서의 이벤트 버블링"
excerpt: "JavaScript의 이벤트 버블링에 대해 알아보고, 이를 활용하는 방법과 막는 방법을 설명합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, DOM, Event Handling]
permalink: /JavaScript/event-bubbling/
toc: true
toc_sticky: true
date: 2024-12-1
last_modified_at: 2024-12-1
---

## 이벤트 버블링(Event Bubbling)란?

이벤트 버블링은 **DOM 트리에서 특정 요소에 이벤트가 발생하면, 그 이벤트가 부모 요소를 거슬러 올라가며 전파되는 현상**을 말합니다. 즉, 이벤트가 가장 안쪽의 요소(이벤트가 실제로 발생한 요소, `event.target`)에서 시작하여 점차 바깥쪽의 부모 요소들로 전달됩니다.

---

## 동작 방식

1. **이벤트 발생**: 사용자가 특정 DOM 요소에서 클릭(click), 키보드 입력(keypress), 포커스(focus) 등의 이벤트를 발생시킴.
2. **이벤트 전파**:
   - **캡처링 단계(Capturing Phase)**: 이벤트가 루트(`document`)에서 시작하여 이벤트가 발생한 요소까지 내려오는 단계.
   - **타깃 단계(Target Phase)**: 이벤트가 실제로 발생한 요소에서 실행되는 단계.
   - **버블링 단계(Bubbling Phase)**: 이벤트가 발생한 요소에서 시작하여 루트(`document`) 방향으로 부모 요소를 거슬러 올라가는 단계.

이벤트 버블링은 이 중 **버블링 단계**에서 발생하는 동작입니다.

---

## 이벤트 버블링의 예시

HTML 구조:
``` html
<div id="parent">
  <button id="child">Click Me</button>
</div>
```

JavaScript 코드:
``` js
document.getElementById('parent').addEventListener('click', () => {
  alert('Parent clicked!');
});

document.getElementById('child').addEventListener('click', () => {
  alert('Child clicked!');
});
```

1. 사용자가 **Child 버튼**을 클릭하면, 다음 순서로 이벤트가 발생합니다:
   - `#child`의 클릭 이벤트 핸들러 실행 → "Child clicked!" 알림 표시.
   - 이벤트가 부모인 `#parent`로 버블링 → `#parent`의 클릭 이벤트 핸들러 실행 → "Parent clicked!" 알림 표시.

---

## 이벤트 버블링을 막는 방법

버블링을 중단하고 싶을 때는 **`event.stopPropagation()`** 메서드를 사용합니다.

``` js
document.getElementById('child').addEventListener('click', (event) => {
  event.stopPropagation(); // 부모로 이벤트 전파를 중단
  alert('Child clicked!');
});
```

이제 `#child`의 클릭 이벤트는 실행되지만, `#parent`의 클릭 이벤트는 실행되지 않습니다.

---

## 이벤트 버블링 활용 사례

1. **이벤트 위임(Event Delegation)**:
   - 많은 하위 요소에 각각 이벤트를 등록하는 대신, 상위 요소에 이벤트를 등록하여 하위 요소의 이벤트를 처리.
   - 예: 동적으로 추가된 리스트 항목에 클릭 이벤트를 처리할 때.
   
   ``` js
   document.getElementById('parent').addEventListener('click', (event) => {
     if (event.target.tagName === 'BUTTON') {
       alert(`${event.target.textContent} clicked!`);
     }
   });
   ```

2. **전체 UI 제어**:
   - 클릭, 키 입력 등의 이벤트를 루트 요소에서 한 번에 처리하여 코드 간결화.

---

## 버블링 단계의 특징

- 이벤트가 발생한 요소부터 루트로 전파.
- 모든 부모 요소에서 이벤트 핸들러가 실행될 수 있음.
- 기본 동작과 상관없이 실행되므로 주의가 필요함(예: `a` 태그의 기본 동작).

---

## 요약

- 이벤트 버블링은 이벤트가 DOM 트리를 따라 위쪽으로 전파되는 현상.
- `event.stopPropagation()`으로 버블링을 막을 수 있음.
- 효율적인 이벤트 처리를 위해 이벤트 위임과 함께 자주 사용됨.
