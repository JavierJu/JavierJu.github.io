---
title: "JavaScript 이벤트 위임 (Event Delegation) 이해하기"
excerpt: "이벤트 위임을 활용하여 성능을 최적화하고 동적 요소를 효율적으로 처리하는 방법을 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, DOM, Event Delegation, Event Handling]
permalink: /JavaScript/event-delegation/
toc: true
toc_sticky: true
date: 2024-12-1
last_modified_at: 2024-12-1
---

## 이벤트 위임 (Event Delegation)란?

이벤트 위임은 이벤트 핸들러를 여러 개의 자식 요소에 각각 부착하지 않고, **공통 조상 요소에 이벤트 핸들러를 부착**하여 이벤트를 처리하는 기법입니다. 이는 **이벤트 버블링**(Event Bubbling)을 활용한 패턴으로, 자식 요소에서 발생한 이벤트가 상위 요소로 전파되는 특징을 이용합니다.

---

## 동작 원리

1. 이벤트가 발생하면, DOM의 **이벤트 버블링** 메커니즘에 따라 이벤트가 상위 요소로 전파됩니다.
2. 조상 요소에 등록된 핸들러가 이 이벤트를 감지합니다.
3. 핸들러 내부에서 `event.target`을 사용해 실제 이벤트가 발생한 요소(타겟)를 식별하고, 특정 조건에 따라 동작을 수행합니다.

---

## 장점

1. **효율성 증가**  
   많은 자식 요소에 각각 이벤트 핸들러를 추가할 필요가 없으므로, 메모리와 성능 면에서 효율적입니다.

2. **동적 요소 처리**  
   DOM이 동적으로 변경되거나 새로운 자식 요소가 추가되어도, 별도의 이벤트 핸들러를 추가하지 않아도 동작합니다.

3. **코드 간결화**  
   한 곳에서 모든 자식 요소의 이벤트를 처리할 수 있어 관리가 편리합니다.

---

## 구현 예제

### 1. 자식 요소마다 이벤트 핸들러 부착
```js
const buttons = document.querySelectorAll('.button');

buttons.forEach(button => {
    button.addEventListener('click', () => {
        console.log('Button clicked:', button.textContent);
    });
});
```

### 2. 이벤트 위임을 사용한 구현
```js
const container = document.querySelector('.container');

container.addEventListener('click', event => {
    if (event.target.classList.contains('button')) {
        console.log('Button clicked:', event.target.textContent);
    }
});
```
- **`event.target`**: 실제 이벤트가 발생한 요소.
- **`event.currentTarget`**: 이벤트 핸들러가 등록된 요소 (여기서는 `container`).

---

## 사용 예시

### 1. 동적으로 추가된 요소 처리
```js
const list = document.querySelector('#list');

// 이벤트 위임
list.addEventListener('click', event => {
    if (event.target.tagName === 'LI') {
        alert(`You clicked on: ${event.target.textContent}`);
    }
});

// 새로운 리스트 아이템 추가
const newItem = document.createElement('li');
newItem.textContent = 'New Item';
list.appendChild(newItem);
```
동적으로 생성된 `li` 요소도 이벤트 위임 덕분에 문제 없이 처리할 수 있습니다.

### 2. 폼 입력 유효성 검사
```js
document.querySelector('form').addEventListener('input', event => {
    const { name, value } = event.target;
    console.log(`Input changed: ${name} = ${value}`);
});
```

---

## 이벤트 위임의 한계

1. **이벤트 버블링이 지원되지 않는 이벤트**  
   - `focus`, `blur`와 같은 이벤트는 버블링되지 않으므로 이벤트 위임이 불가능합니다.
   - 이를 해결하려면 **캡처링**(Event Capturing)을 사용할 수도 있습니다.

2. **복잡한 조건 처리**  
   - 특정 조건에 따라 처리해야 할 경우, 조건문이 복잡해질 수 있습니다.

---

## 요약

- 이벤트 위임은 부모 요소에 이벤트 핸들러를 등록하여 자식 요소에서 발생하는 이벤트를 처리하는 기법입니다.
- **효율성**과 **유연성**을 제공하며, 특히 동적으로 생성된 요소를 처리하는 데 유용합니다.
- `event.target`을 활용하여 이벤트가 발생한 실제 요소를 식별합니다.
