---
title: "JavaScript의 DOM 이벤트에서 event와 this 키워드의 관계"
excerpt: "DOM 이벤트에서 event 객체와 this 키워드가 어떻게 동작하고 상호작용하는지, 그리고 상황에 따라 어떻게 다르게 동작하는지에 대해 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, DOM, Event Handling, this, event]
permalink: /JavaScript/dom-event-this/
toc: true
toc_sticky: true
date: 2024-12-1
last_modified_at: 2024-12-1
---

## JavaScript의 DOM 이벤트에서 `event`와 `this` 키워드

JavaScript의 DOM 이벤트를 다룰 때, **`event` 객체**와 **`this` 키워드**는 자주 사용됩니다. 두 키워드는 서로 밀접하게 관련되어 있으며, 이벤트 핸들러의 문맥(Context)에 따라 다르게 동작합니다. 이 글에서는 DOM 이벤트 핸들러에서의 `event`와 `this`의 관계를 살펴보고, 상황별로 어떻게 사용되는지 알아보겠습니다.

---

### `event` 객체란?
`event` 객체는 이벤트가 발생할 때 브라우저가 생성하여 이벤트 핸들러에 전달하는 객체입니다. 이를 통해 이벤트에 대한 다양한 정보를 얻을 수 있습니다.

#### 주요 특징
- 이벤트의 **타입**(예: `click`, `input`)과 관련 정보를 제공합니다.
- 이벤트가 발생한 **대상 요소**를 참조할 수 있는 **`target`** 속성을 포함합니다.
- 기본 동작을 막거나 이벤트 전파를 제어하는 메서드(`preventDefault`, `stopPropagation` 등)를 제공합니다.

---

### `this` 키워드란?
**`this` 키워드**는 자바스크립트에서 문맥(Context)을 나타냅니다. DOM 이벤트 핸들러에서는 기본적으로 **이벤트 리스너가 연결된 요소**를 참조합니다.

---

### `event`와 `this`의 관계

#### (1) 기본 이벤트 핸들러
기본적으로, DOM 이벤트 핸들러에서 `this`는 이벤트가 연결된 요소를, `event.target`은 실제 이벤트가 발생한 요소를 참조합니다.

```javascript
document.getElementById('myButton').addEventListener('click', function(event) {
    console.log(this);         // 이벤트 핸들러가 연결된 요소 (myButton)
    console.log(event.target); // 이벤트가 실제로 발생한 요소 (myButton)
});
```

#### (2) 화살표 함수 사용
화살표 함수에서는 **`this`가 정적으로 바인딩**되어 이벤트 리스너와 연결된 요소를 참조하지 않습니다. 대신, 상위 스코프의 `this`를 사용합니다.

```javascript
document.getElementById('myButton').addEventListener('click', (event) => {
    console.log(this);         // 상위 스코프의 this (예: window)
    console.log(event.target); // 이벤트가 발생한 요소 (myButton)
});
```

#### (3) 이벤트 위임과의 차이
이벤트 위임에서는 이벤트 리스너가 부모 요소에 연결되고, 자식 요소에서 이벤트가 발생합니다. 이 경우 `this`와 `event.target`은 서로 다른 요소를 참조합니다.

```javascript
document.getElementById('parentDiv').addEventListener('click', function(event) {
    console.log(this);         // 이벤트 리스너가 연결된 요소 (parentDiv)
    console.log(event.target); // 이벤트가 발생한 실제 요소 (예: 클릭된 자식 요소)
});
```

---

### `this`를 명시적으로 바인딩하기
`this`를 명시적으로 다른 객체에 바인딩하려면 `bind()` 메서드를 사용할 수 있습니다.

```javascript
const myButton = document.getElementById('myButton');

myButton.addEventListener('click', function(event) {
    console.log(this); // 기본적으로 myButton
}.bind(document)); // this를 document로 강제로 바인딩
```

---

### 요약
1. **`event` 객체**는 이벤트에 대한 정보를 담고 있으며, `event.target`을 통해 실제 이벤트가 발생한 요소를 참조할 수 있습니다.
2. **`this` 키워드**는 기본적으로 이벤트 리스너가 연결된 요소를 참조하지만, 화살표 함수나 명시적 바인딩에 따라 달라질 수 있습니다.
3. **`event.target`**과 **`this`**를 활용해 이벤트 버블링과 위임을 효율적으로 처리할 수 있습니다.

DOM 이벤트를 다룰 때, `event`와 `this`를 올바르게 이해하면 더 직관적이고 효율적인 코드를 작성할 수 있습니다.
