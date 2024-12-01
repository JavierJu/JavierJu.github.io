---
title: "JavaScript 이벤트 객체와 키보드 이벤트의 이해"
excerpt: "JavaScript의 이벤트 객체와 키보드 이벤트의 작동 방식, 주요 프로퍼티 및 활용 예제를 정리합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Event, Keyboard, Web Development]
permalink: /JavaScript/event-object-keyboard-event/
toc: true
toc_sticky: true
date: 2024-12-1
last_modified_at: 2024-12-1
---

## JavaScript 이벤트 객체와 키보드 이벤트의 이해

웹 개발에서 사용자와의 상호작용을 처리하기 위해 JavaScript의 이벤트 객체와 키보드 이벤트는 매우 중요합니다. 이번 글에서는 이벤트 객체와 키보드 이벤트의 작동 원리, 주요 프로퍼티, 그리고 실제 활용 예제를 알아보겠습니다.

---

### 1. 이벤트 객체(Event Object)란?

이벤트 객체는 이벤트가 발생했을 때 브라우저에서 생성하는 객체로, 이벤트에 대한 다양한 정보를 담고 있습니다. 이벤트 핸들러에서 첫 번째 매개변수로 접근할 수 있습니다.

#### 주요 프로퍼티와 메서드
- **`type`**  
  이벤트의 유형을 반환합니다.
  ```js
  element.addEventListener('click', (event) => {
      console.log(event.type); // 'click'
  });
  ```
  
- **`target`**  
  이벤트가 발생한 요소를 반환합니다.
  ```js
  element.addEventListener('click', (event) => {
      console.log(event.target); // 클릭된 요소
  });
  ```

- **`currentTarget`**  
  이벤트 리스너가 부착된 요소를 반환합니다.
  ```js
  element.addEventListener('click', (event) => {
      console.log(event.currentTarget); // 리스너가 붙은 요소
  });
  ```

- **`preventDefault()`**  
  기본 동작(예: 링크 클릭 시 페이지 이동)을 방지합니다.
  ```js
  link.addEventListener('click', (event) => {
      event.preventDefault();
  });
  ```

- **`stopPropagation()`**  
  이벤트 전파를 중지합니다.  
  ```js
  button.addEventListener('click', (event) => {
      event.stopPropagation();
  });
  ```

- **좌표 관련 프로퍼티**  
  - `clientX`, `clientY`: 클릭 지점의 x, y 좌표(뷰포트 기준)를 반환합니다.
  ```js
  element.addEventListener('click', (event) => {
      console.log(event.clientX, event.clientY); // 클릭 위치
  });
  ```

---

### 2. 키보드 이벤트(Keyboard Event)

키보드 이벤트는 사용자가 키보드를 눌렀을 때 발생하며, `keydown`, `keyup`, `keypress` 이벤트가 있습니다.  

#### 키보드 이벤트 유형
1. **`keydown`**  
   키를 누를 때 발생합니다.
   ```js
   document.addEventListener('keydown', (event) => {
       console.log(`Key down: ${event.key}`);
   });
   ```

2. **`keyup`**  
   키에서 손을 뗄 때 발생합니다.
   ```js
   document.addEventListener('keyup', (event) => {
       console.log(`Key up: ${event.key}`);
   });
   ```

3. **`keypress`** (권장되지 않음)  
   키를 누를 때 발생합니다. 현재는 `keydown` 사용이 권장됩니다.

---

#### 키보드 이벤트 객체의 주요 프로퍼티
- **`key`**  
  눌린 키의 이름을 문자열로 반환합니다.
  ```js
  document.addEventListener('keydown', (event) => {
      console.log(`Key pressed: ${event.key}`); // 예: 'a', 'Enter'
  });
  ```

- **`code`**  
  물리적 키의 코드 값을 반환합니다.
  ```js
  document.addEventListener('keydown', (event) => {
      console.log(`Key code: ${event.code}`); // 예: 'KeyA', 'Space'
  });
  ```

- **조합키 상태 확인**  
  `altKey`, `ctrlKey`, `shiftKey`, `metaKey`는 해당 키가 눌린 상태인지 반환합니다.
  ```js
  document.addEventListener('keydown', (event) => {
      if (event.ctrlKey && event.key === 's') {
          event.preventDefault();
          console.log('Save shortcut triggered');
      }
  });
  ```

- **`repeat`**  
  키를 누른 상태로 반복 입력 중인지 확인할 수 있습니다.
  ```js
  document.addEventListener('keydown', (event) => {
      console.log(`Is key repeating? ${event.repeat}`);
  });
  ```

---

### 활용 예제

#### 특정 키 조합 처리
```js
document.addEventListener('keydown', (event) => {
    if (event.ctrlKey && event.key === 'z') {
        console.log('Undo operation triggered');
    }
});
```

#### 입력 필터링: 숫자만 허용
```js
input.addEventListener('keydown', (event) => {
    if (!/^[0-9]$/.test(event.key) && event.key !== 'Backspace') {
        event.preventDefault();
    }
});
```

---

### 마무리

JavaScript의 이벤트 객체와 키보드 이벤트는 사용자 입력을 처리하는 핵심 기술입니다. 다양한 프로퍼티와 메서드를 활용해 보다 직관적이고 유연한 사용자 경험을 제공할 수 있습니다. 실습을 통해 익숙해지면 더욱 효과적으로 사용할 수 있을 것입니다! 🚀
