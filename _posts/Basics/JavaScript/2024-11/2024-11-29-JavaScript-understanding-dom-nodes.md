---
title: "DOM 노드란 무엇인가? 노드와 요소의 차이 이해하기"
excerpt: "DOM의 기본 개념인 노드와 요소의 차이를 이해하고, 노드가 필요한 상황과 DOM 트리 탐색 방법에 대해 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, HTML, DOM, Web Development]
permalink: /JavaScript/understanding-dom-nodes/
toc: true
toc_sticky: true
date: 2024-11-29
last_modified_at: 2024-11-29
---

HTML 문서를 다룰 때 DOM(Document Object Model)은 필수적인 개념입니다. 이 글에서는 DOM의 기본 단위인 **노드**가 무엇인지, 요소와는 어떻게 다른지, 그리고 이를 어떻게 활용할 수 있는지 알아보겠습니다.

---

## 노드란?

노드는 DOM에서 HTML 문서를 구성하는 기본 단위입니다. HTML의 모든 요소와 콘텐츠는 노드로 표현되며, 각각의 노드는 특정한 타입을 가집니다.

### **노드의 주요 타입**

1. **요소 노드 (Element Node)**  
   HTML 태그 자체를 나타냅니다.  
   **예시**: `<div>`는 요소 노드입니다.

2. **텍스트 노드 (Text Node)**  
   요소 안의 텍스트 콘텐츠를 나타냅니다.  
   **예시**: `<div>Hello</div>`에서 "Hello"가 텍스트 노드입니다.

3. **주석 노드 (Comment Node)**  
   HTML 주석을 나타냅니다.  
   **예시**: `<!-- Comment -->`

4. **문서 노드 (Document Node)**  
   전체 HTML 문서를 나타내는 최상위 노드입니다.

5. **속성 노드 (Attribute Node)**  
   HTML 요소의 속성을 나타냅니다. 일반적으로 DOM 탐색에서는 잘 사용되지 않습니다.

---

## 노드 vs 요소

### 주요 차이점

| **특징**            | **노드**                                    | **요소**                              |
|---------------------|---------------------------------------------|---------------------------------------|
| **포괄 범위**       | 모든 노드 유형 (요소, 텍스트, 주석 등) 포함 | HTML 태그에 해당하는 노드만 포함      |
| **예시**            | `<div>Hello</div>`: `<div>`와 텍스트 "Hello" 둘 다 노드 | `<div>`만 요소                       |
| **사용 프로퍼티**   | `childNodes`, `parentNode`                 | `children`, `parentElement`          |

### 차이점 예제

HTML 코드:
``` html
<div>
  Hello
  <!-- This is a comment -->
</div>
```

JavaScript 코드:
``` js
const div = document.querySelector('div');

// 모든 자식 노드 가져오기 (텍스트, 주석 포함)
console.log(div.childNodes);
// 출력: NodeList [Text, Text, Comment]

// 요소 노드만 가져오기
console.log(div.children);
// 출력: HTMLCollection []
```

---

## 노드를 사용하는 상황

노드는 텍스트 노드나 주석 노드까지 포함한 DOM 트리의 세부적인 탐색과 조작이 필요할 때 유용합니다.

### **텍스트 노드 작업**
HTML 코드:
``` html
<div>Hello</div>
```

JavaScript 코드:
``` js
const div = document.querySelector('div');
const textNode = div.childNodes[0]; // 텍스트 노드 가져오기
console.log(textNode.nodeValue); // "Hello"

textNode.nodeValue = "Hi"; // 텍스트 수정
console.log(div.innerHTML); // "<div>Hi</div>"
```

### **주석 노드 탐색**
HTML 코드:
``` html
<div>
  <!-- Important comment -->
</div>
```

JavaScript 코드:
``` js
const div = document.querySelector('div');
const commentNode = div.childNodes[0]; // 주석 노드 가져오기
console.log(commentNode.nodeType); // 8 (주석 노드의 타입)
console.log(commentNode.nodeValue); // "Important comment"
```

---

## 노드 탐색에 사용하는 프로퍼티와 메서드

### 주요 프로퍼티
1. **`childNodes`**  
   - 요소의 모든 자식 노드를 가져옵니다. (텍스트, 주석 포함)  
   **예시**: `div.childNodes`

2. **`parentNode`**  
   - 현재 노드의 부모 노드를 반환합니다.  
   **예시**: `div.parentNode`

3. **`firstChild` / `lastChild`**  
   - 첫 번째 또는 마지막 자식 노드를 반환합니다.  
   **예시**: `div.firstChild`

4. **`nextSibling` / `previousSibling`**  
   - 같은 계층에 있는 다음 또는 이전 노드를 반환합니다.  
   **예시**: `div.nextSibling`

### 노드 타입 구분하기
`nodeType` 속성을 사용하여 노드의 타입을 확인할 수 있습니다.

| **nodeType 값** | **설명**                  | **예시**                          |
|-----------------|---------------------------|------------------------------------|
| 1               | 요소 노드                 | `<div>`, `<p>`, `<span>` 등       |
| 3               | 텍스트 노드               | 텍스트 콘텐츠                     |
| 8               | 주석 노드                 | `<!-- Comment -->`                |
| 9               | 문서 노드                 | `document` 객체                   |

---

## 노드를 사용할 때와 요소만 사용할 때

### 요소만 필요할 때
- 텍스트 노드와 주석을 무시하고, HTML 태그를 기준으로 작업할 때.
- **예시**: HTML 레이아웃 조작, UI 구성.

### 노드가 필요할 때
- 텍스트나 주석 콘텐츠를 포함한 세부적인 DOM 트리 작업이 필요할 때.
- **예시**: 텍스트 수정, 주석 내용 분석.

---

## 결론

DOM에서 **노드**는 HTML 문서를 구성하는 모든 요소를 아우르는 기본 단위입니다.  
일반적인 작업에서는 **요소 노드**만 사용하지만, 텍스트나 주석까지 다룰 필요가 있을 때는 노드를 활용하세요. 이를 통해 DOM 트리를 더 세밀하고 유연하게 탐색하고 조작할 수 있습니다.
