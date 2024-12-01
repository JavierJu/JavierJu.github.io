---
title: "DOM 이벤트에 대한 완벽 가이드"
excerpt: "DOM 이벤트의 기본 개념부터 이벤트 리스너, 전파, 위임까지 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, DOM, Web Development, Event Handling, Frontend]
permalink: /JavaScript/dom-events-guide/
toc: true
toc_sticky: true
date: 2024-12-1
last_modified_at: 2024-12-1
---

## DOM 이벤트란?

DOM 이벤트는 웹 페이지에서 발생하는 사용자의 상호작용을 처리하기 위해 사용됩니다. 예를 들어, 버튼 클릭, 텍스트 입력, 마우스 이동 등과 같은 사용자 행동은 모두 DOM 이벤트로 간주됩니다.

## 이벤트 종류

### 1. 마우스 이벤트
- `click`: 마우스를 클릭했을 때 발생
- `dblclick`: 마우스를 더블 클릭했을 때 발생
- `mouseover`: 마우스가 요소 위로 이동했을 때 발생
- `mouseout`: 마우스가 요소를 벗어났을 때 발생
- `mousemove`: 마우스가 움직일 때 발생

### 2. 키보드 이벤트
- `keydown`: 키를 눌렀을 때 발생
- `keypress`: 키가 눌려지고 있는 동안 발생
- `keyup`: 키를 뗄 때 발생

### 3. 폼 이벤트
- `submit`: 폼이 제출될 때 발생
- `input`: 입력 필드의 내용이 변경될 때 발생
- `change`: 폼 값이 변경되었을 때 발생

### 4. 윈도우/문서 이벤트
- `load`: 페이지가 완전히 로드되었을 때 발생
- `resize`: 브라우저 창 크기 변경 시 발생
- `scroll`: 페이지가 스크롤될 때 발생
- `focus`: 요소가 포커스를 받을 때 발생
- `blur`: 요소가 포커스를 잃을 때 발생

## 이벤트 핸들러와 이벤트 리스너

이벤트 핸들러는 특정 이벤트가 발생했을 때 실행되는 함수입니다.

### HTML 속성에서 직접 정의
```html
<button onclick="alert('Button clicked!')">Click Me</button>
```

### JavaScript에서 정의
```js
const button = document.querySelector('button');
button.addEventListener('click', function() {
  alert('Button clicked!');
});
```

## 이벤트 전파 (Event Propagation)

이벤트는 DOM 트리에서 상향(버블링) 또는 하향(캡처링) 방식으로 전파됩니다.

### 버블링 (Bubbling)
이벤트가 가장 하위 요소에서 상위 요소로 전파됩니다.

### 캡처링 (Capturing)
이벤트가 상위 요소에서 하위 요소로 전파됩니다.

### 전파 방지
- `event.stopPropagation()`: 이벤트 전파를 막음
- `event.preventDefault()`: 기본 동작(예: 폼 제출)을 막음

### 캡처링과 버블링 설정
```js
element.addEventListener('click', handler, true); // 캡처링
element.addEventListener('click', handler, false); // 버블링 (기본값)
```

## 이벤트 객체 (Event Object)

이벤트가 발생하면 이벤트 객체가 생성되어 추가 정보를 제공합니다.

```js
button.addEventListener('click', function(event) {
  console.log(event.target); // 이벤트가 발생한 요소
  console.log(event.type);   // 이벤트 유형 (e.g., 'click')
});
```

## 이벤트 위임 (Event Delegation)

부모 요소에 이벤트 리스너를 등록하여 자식 요소들의 이벤트를 처리하는 방식입니다.

```js
document.querySelector('#parent').addEventListener('click', function(event) {
  if (event.target && event.target.matches('button.classname')) {
    alert('Button clicked!');
  }
});
```

## 비동기 이벤트

일부 이벤트는 비동기적으로 처리됩니다. 예를 들어, `setTimeout`, `setInterval`, AJAX 요청 등이 이에 해당합니다.

## 유용한 메서드와 옵션

### 이벤트 리스너 제거
```js
element.removeEventListener('click', handler);
```

### 한 번만 실행되는 이벤트
```js
element.addEventListener('click', handler, { once: true });
```

## 결론

DOM 이벤트는 웹 페이지와 사용자 간 상호작용을 다루는 강력한 도구입니다. 이벤트를 적절히 활용하면 보다 직관적이고 반응성이 뛰어난 웹 애플리케이션을 만들 수 있습니다.
