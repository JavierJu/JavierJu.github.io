---
title: "JavaScript DOM 계층 이동: parentElement, child, sibling 활용법"
excerpt: "JavaScript에서 DOM 요소 간의 계층을 탐색하고 조작하는 방법에 대해 알아봅니다. parentElement, children, sibling 등 주요 프로퍼티와 메서드를 정리하고 실용적인 사용 사례를 소개합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, DOM, Web Development, HTML]
permalink: /JavaScript/dom-navigation/
toc: true
toc_sticky: true
date: 2024-11-29
last_modified_at: 2024-11-29
---

JavaScript를 사용하면 DOM(Document Object Model)을 통해 HTML 문서 구조를 탐색하고 특정 요소를 쉽게 조작할 수 있습니다. DOM 계층 이동과 관련된 주요 프로퍼티와 메서드를 정리하여, 실용적인 사용 방법을 소개합니다.

## 1. 부모 요소 접근
### **`parentElement`**
- **설명**: 현재 요소의 바로 위 부모 요소를 반환합니다. 부모가 없으면 `null`을 반환합니다.
- **사용 예시**:
``` js
const child = document.querySelector('.child');
const parent = child.parentElement;
console.log(parent); // .parent 요소 출력
```

### **`parentNode`**
- **설명**: 부모 노드를 반환합니다. 텍스트 노드 등도 포함될 수 있습니다.
- **차이점**: 요소 중심 탐색에서는 `parentElement`를 선호합니다.

---

## 2. 자식 요소 접근
### **`children`**
- **설명**: 현재 요소의 모든 자식 요소를 HTMLCollection으로 반환합니다.
- **특징**: 텍스트 노드는 포함하지 않습니다.
- **사용 예시**:
``` js
const parent = document.querySelector('.parent');
const childElements = parent.children;
console.log(childElements); // 자식 요소들의 컬렉션 출력
```

### **`childNodes`**
- **설명**: 현재 노드의 모든 자식 노드를 반환합니다 (텍스트 노드, 주석 포함).
- **사용 예시**:
``` js
const parent = document.querySelector('.parent');
const childNodes = parent.childNodes;
console.log(childNodes); // 자식 노드들 출력 (텍스트, 요소 포함)
```

### **`firstElementChild`**
- **설명**: 첫 번째 자식 요소를 반환합니다. 텍스트 노드는 제외됩니다.
- **사용 예시**:
``` js
const parent = document.querySelector('.parent');
const firstChild = parent.firstElementChild;
console.log(firstChild); // 첫 번째 자식 요소 출력
```

### **`lastElementChild`**
- **설명**: 마지막 자식 요소를 반환합니다.
- **사용 예시**:
``` js
const parent = document.querySelector('.parent');
const lastChild = parent.lastElementChild;
console.log(lastChild); // 마지막 자식 요소 출력
```

---

## 3. 형제 요소 접근
### **`nextElementSibling`**
- **설명**: 현재 요소의 다음 형제 요소를 반환합니다.
- **사용 예시**:
``` js
const firstChild = document.querySelector('.child1');
const nextSibling = firstChild.nextElementSibling;
console.log(nextSibling); // .child2 출력
```

### **`previousElementSibling`**
- **설명**: 현재 요소의 이전 형제 요소를 반환합니다.
- **사용 예시**:
``` js
const secondChild = document.querySelector('.child2');
const prevSibling = secondChild.previousElementSibling;
console.log(prevSibling); // .child1 출력
```

---

## 4. 특정 요소로의 접근
### **`querySelector`**
- **설명**: CSS 선택자를 사용하여 특정 요소를 반환합니다.
- **사용 예시**:
``` js
const element = document.querySelector('.className');
console.log(element);
```

### **`querySelectorAll`**
- **설명**: CSS 선택자로 조건에 맞는 모든 요소를 NodeList로 반환합니다.
- **사용 예시**:
``` js
const elements = document.querySelectorAll('.className');
console.log(elements);
```

---

## 5. 부모-자식-형제를 조합한 탐색 예시
``` js
const child = document.querySelector('.child2');

// 부모 찾기
const parent = child.parentElement;
console.log('Parent:', parent);

// 이전 형제 찾기
const prevSibling = child.previousElementSibling;
console.log('Previous Sibling:', prevSibling);

// 다음 형제 찾기
const nextSibling = child.nextElementSibling;
console.log('Next Sibling:', nextSibling);

// 첫 번째 자식 찾기
const firstChild = parent.firstElementChild;
console.log('First Child:', firstChild);

// 마지막 자식 찾기
const lastChild = parent.lastElementChild;
console.log('Last Child:', lastChild);
```

---

## 6. DOM 탐색 프로퍼티 요약

| 프로퍼티                  | 설명                                | 텍스트 노드 포함 여부 |
|---------------------------|-------------------------------------|-----------------------|
| `parentElement`           | 부모 요소 반환                     | 아니오                |
| `parentNode`              | 부모 노드 반환                     | 예                   |
| `children`                | 자식 요소들의 HTMLCollection 반환  | 아니오                |
| `childNodes`              | 자식 노드들의 NodeList 반환        | 예                   |
| `firstElementChild`       | 첫 번째 자식 요소 반환             | 아니오                |
| `lastElementChild`        | 마지막 자식 요소 반환              | 아니오                |
| `nextElementSibling`      | 다음 형제 요소 반환                | 아니오                |
| `previousElementSibling`  | 이전 형제 요소 반환                | 아니오                |

---

JavaScript에서 DOM 계층 탐색은 웹 페이지의 구조를 이해하고 상호작용을 가능하게 하는 중요한 기술입니다. 다양한 프로퍼티를 조합하여 유연한 탐색 로직을 작성해 보세요!
