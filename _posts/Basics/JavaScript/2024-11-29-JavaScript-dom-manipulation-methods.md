---
title: "JavaScript에서 DOM 요소 삽입과 삭제: 주요 메서드와 활용 예제"
excerpt: "DOM 조작을 위한 다양한 JavaScript 메서드(createElement, append, remove 등)를 이해하고 활용하는 방법을 소개합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, DOM, Web Development, Frontend]
permalink: /JavaScript/dom-manipulation-methods/
toc: true
toc_sticky: true
date: 2024-11-29
last_modified_at: 2024-11-29
---

## JavaScript에서 DOM 요소 삽입과 삭제

JavaScript를 사용하면 DOM(Document Object Model)을 조작하여 동적인 웹 페이지를 구현할 수 있습니다. 여기서는 DOM 요소를 생성, 삽입, 삭제하는 다양한 메서드를 자세히 살펴보겠습니다.

### 1. `document.createElement()`
새로운 HTML 요소를 생성합니다.

**사용법**:
``` js
const newElement = document.createElement('tagName');
```

**예제**:
``` js
const div = document.createElement('div'); // <div></div>
div.textContent = 'Hello World!';         // <div>Hello World!</div>
document.body.appendChild(div);           // 문서의 body에 추가
```

---

### 2. `append()`
부모 요소에 텍스트와 자식 요소를 동시에 추가할 수 있습니다.

**사용법**:
``` js
parentElement.append(element1, element2, 'text');
```

**예제**:
``` js
const parent = document.getElementById('parent');
const span = document.createElement('span');
span.textContent = 'I am a span element.';
parent.append(span, 'Some text'); // 부모 요소에 span과 텍스트 추가
```

---

### 3. `appendChild()`
요소를 부모의 마지막 자식으로 추가합니다.

**사용법**:
``` js
parentElement.appendChild(newChild);
```

**예제**:
``` js
const ul = document.createElement('ul');
const li = document.createElement('li');
li.textContent = 'List Item 1';
ul.appendChild(li);
document.body.appendChild(ul);
```

---

### 4. `removeChild()`
자식 요소를 제거합니다.

**사용법**:
``` js
parentElement.removeChild(childElement);
```

**예제**:
``` js
const parent = document.getElementById('parent');
const child = document.getElementById('child');
parent.removeChild(child); // child 제거
```

---

### 5. `remove()`
요소 자체를 제거합니다. 부모를 참조할 필요가 없습니다.

**사용법**:
``` js
element.remove();
```

**예제**:
``` js
const element = document.getElementById('element');
element.remove(); // 자신을 DOM에서 제거
```

---

### 6. `insertAdjacentElement()`
특정 위치에 요소를 삽입합니다.

**사용법**:
``` js
referenceElement.insertAdjacentElement(position, newElement);
```

`position` 값:
- `'beforebegin'`: 참조 요소 바로 앞
- `'afterbegin'`: 참조 요소 내부의 첫 번째 자식 앞
- `'beforeend'`: 참조 요소 내부의 마지막 자식 뒤
- `'afterend'`: 참조 요소 바로 뒤

**예제**:
``` js
const reference = document.getElementById('reference');
const newElement = document.createElement('div');
newElement.textContent = 'New Element';
reference.insertAdjacentElement('beforebegin', newElement);
```

---

### 7. `innerHTML`
HTML 문자열로 요소의 내용을 설정하거나 업데이트합니다.

**사용법**:
``` js
element.innerHTML = '<tag>content</tag>';
```

**예제**:
``` js
const container = document.getElementById('container');
container.innerHTML = '<p>New content</p>';
```

---

### 8. `replaceChild()`
기존 자식 요소를 새로운 요소로 교체합니다.

**사용법**:
``` js
parentElement.replaceChild(newChild, oldChild);
```

**예제**:
``` js
const parent = document.getElementById('parent');
const newChild = document.createElement('div');
newChild.textContent = 'I am new';
const oldChild = document.getElementById('old');
parent.replaceChild(newChild, oldChild);
```

---

### 9. `cloneNode()`
요소를 복제합니다.

**사용법**:
``` js
const clone = element.cloneNode(deep);
```

**예제**:
``` js
const original = document.getElementById('original');
const copy = original.cloneNode(true);
document.body.appendChild(copy);
```

---

### 10. `insertBefore()`
특정 위치에 요소를 삽입합니다.

**사용법**:
``` js
parentElement.insertBefore(newChild, referenceChild);
```

**예제**:
``` js
const parent = document.getElementById('parent');
const newChild = document.createElement('div');
newChild.textContent = 'New Child';
const referenceChild = document.getElementById('reference');
parent.insertBefore(newChild, referenceChild);
```

---

### 11. 템플릿을 활용한 삽입
`<template>` 태그를 사용하여 미리 정의된 구조를 삽입할 수 있습니다.

**예제**:
``` html
<template id="myTemplate">
  <div class="template-content">
    <p>Template content here!</p>
  </div>
</template>
```

``` js
const template = document.getElementById('myTemplate');
const clone = template.content.cloneNode(true);
document.body.appendChild(clone);
```

---

### 12. `outerHTML`
요소 자체를 포함하여 HTML 문자열로 대체합니다.

**사용법**:
``` js
element.outerHTML = '<div>New Element</div>';
```

**예제**:
``` js
const element = document.getElementById('element');
element.outerHTML = '<p>Replaced Element</p>';
```

---

### 요약
다양한 메서드를 적절히 조합하면 효율적으로 DOM을 조작할 수 있습니다. 각 메서드의 특징과 사용 사례를 잘 이해하고 프로젝트에 맞게 활용해 보세요.
