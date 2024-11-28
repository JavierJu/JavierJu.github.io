---
title: "JavaScript DOM 선택 메서드 총정리: getElementById부터 querySelectorAll까지"
excerpt: "JavaScript에서 DOM 요소를 선택하는 다양한 메서드(getElementById, getElementsByClassName, querySelector 등)의 특징과 사용법을 예제와 함께 정리합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, DOM, Web Development]
permalink: /JavaScript/dom-selection-methods/
toc: true
toc_sticky: true
date: 2024-11-28
last_modified_at: 2024-11-28
---

JavaScript에서 DOM(Document Object Model) 요소를 선택하고 조작하는 것은 웹 개발의 핵심입니다. 이 글에서는 주요 DOM 선택 메서드의 차이점과 사용법을 예제와 함께 정리하겠습니다.

## 1. `getElementById`
**설명**: HTML 문서에서 `id` 속성을 기준으로 단일 요소를 선택합니다.  
**특징**:
- 반환값: 단일 요소
- 동일한 `id`를 가진 요소가 여러 개 있을 경우 첫 번째 요소만 반환됩니다.

**예제**:
HTML:
``` html
<div id="example">Hello, World!</div>
```

JavaScript:
``` js
const element = document.getElementById("example");
console.log(element.textContent); // "Hello, World!"
element.style.color = "blue"; // 글자 색을 파란색으로 변경
```

---

## 2. `getElementsByClassName`
**설명**: HTML 문서에서 `class` 이름을 기준으로 여러 요소를 선택합니다.  
**특징**:
- 반환값: HTMLCollection (유사 배열)
- 동일한 클래스를 가진 모든 요소를 선택

**예제**:
HTML:
``` html
<div class="item">Item 1</div>
<div class="item">Item 2</div>
<div class="item">Item 3</div>
```

JavaScript:
``` js
const elements = document.getElementsByClassName("item");
console.log(elements.length); // 3
for (let element of elements) {
  element.style.color = "red"; // 모든 요소의 글자 색을 빨간색으로 변경
}
```

---

## 3. `getElementsByTagName`
**설명**: HTML 문서에서 태그 이름을 기준으로 여러 요소를 선택합니다.  
**특징**:
- 반환값: HTMLCollection
- 태그 이름으로 범용적인 검색 가능

**예제**:
HTML:
``` html
<p>Paragraph 1</p>
<p>Paragraph 2</p>
<p>Paragraph 3</p>
```

JavaScript:
``` js
const paragraphs = document.getElementsByTagName("p");
console.log(paragraphs.length); // 3
for (let i = 0; i < paragraphs.length; i++) {
  paragraphs[i].style.fontWeight = "bold"; // 모든 <p> 태그의 글자를 굵게
}
```

---

## 4. `querySelector`
**설명**: CSS 선택자를 사용하여 첫 번째 요소를 선택합니다.  
**특징**:
- 반환값: 단일 요소
- `id`, `class`, `태그` 또는 복합 CSS 선택자 지원

**예제**:
HTML:
``` html
<div class="container">
  <p id="first">First paragraph</p>
  <p class="highlight">Second paragraph</p>
</div>
```

JavaScript:
``` js
const firstParagraph = document.querySelector("#first");
console.log(firstParagraph.textContent); // "First paragraph"

const highlightedParagraph = document.querySelector(".highlight");
highlightedParagraph.style.backgroundColor = "yellow"; // 배경색을 노란색으로 변경
```
---

## 5. `querySelectorAll`
**설명**: CSS 선택자를 사용하여 모든 일치하는 요소를 선택합니다.  
**특징**:
- 반환값: NodeList (유사 배열, `forEach` 사용 가능)
- CSS 선택자 문법을 활용해 유연한 검색 가능

**예제**:
HTML:
``` html
<div class="container">
  <p class="text">Text 1</p>
  <p class="text">Text 2</p>
  <p class="text">Text 3</p>
</div>
```

JavaScript:
``` js
const texts = document.querySelectorAll(".text");
console.log(texts.length); // 3
texts.forEach((text) => {
  text.style.color = "green"; // 모든 .text 요소의 글자 색을 초록색으로 변경
});
```

---

## 메서드 비교

| 메서드                   | 반환값                | 검색 기준          | 유연성         |
|--------------------------|-----------------------|--------------------|----------------|
| `getElementById`         | 단일 요소             | `id`              | 제한적          |
| `getElementsByClassName` | HTMLCollection        | `class`           | 다수 선택 가능 |
| `getElementsByTagName`   | HTMLCollection        | 태그 이름         | 다수 선택 가능 |
| `querySelector`          | 단일 요소             | CSS 선택자        | 매우 유연       |
| `querySelectorAll`       | NodeList             | CSS 선택자        | 매우 유연       |

---

## 요약
- **단일 요소**를 찾을 때: `getElementById` 또는 `querySelector`
- **다수의 요소**를 찾을 때: `getElementsByClassName`, `getElementsByTagName`, 또는 `querySelectorAll`
- **복잡한 선택**이 필요한 경우: `querySelector`와 `querySelectorAll` 사용 추천

JavaScript DOM 선택 메서드를 잘 활용하면 효율적이고 유연한 웹 개발이 가능합니다. 상황에 맞는 메서드를 선택해 보세요!
