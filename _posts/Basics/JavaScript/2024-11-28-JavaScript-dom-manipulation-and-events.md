---
title: "JavaScript로 DOM 조작과 이벤트 처리 자세히 알아보기"
excerpt: "JavaScript의 DOM(Document Object Model) 조작과 이벤트 처리 방법을 배우고 웹 페이지 요소를 동적으로 제어하는 방법을 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, DOM, HTML, Web Development]
permalink: /JavaScript/dom-manipulation-and-events/
toc: true
toc_sticky: true
date: 2024-11-28
last_modified_at: 2024-11-28
---

## JavaScript의 DOM(Document Object Model)

DOM은 HTML 또는 XML 문서를 프로그램적으로 다룰 수 있도록 문서의 구조를 객체로 표현한 인터페이스입니다. JavaScript를 사용하면 웹 페이지의 요소를 동적으로 수정하거나, 새로 추가하거나, 삭제할 수 있습니다.

---

### DOM의 기본 구조

DOM은 계층적인 트리 구조로 이루어져 있으며, 주요 구성 요소는 다음과 같습니다:

- **Document**: 최상위 객체로, 웹 페이지 전체를 나타냅니다.
- **Element**: HTML 태그를 나타냅니다. 예를 들어 `<div>`, `<p>`, `<img>` 등이 포함됩니다.
- **Text Node**: HTML 요소 내부의 텍스트를 나타냅니다.
- **Attributes**: HTML 요소의 속성을 나타냅니다. 예: `class="example"`.

---

### DOM 관련 주요 객체 및 메서드

#### 1. **Document 객체**
`document` 객체는 DOM 트리의 최상위 객체입니다.

**주요 메서드:**

- `document.getElementById(id)`: 특정 ID를 가진 요소를 반환합니다.
- `document.querySelector(selector)`: CSS 선택자를 사용하여 첫 번째 일치 요소를 반환합니다.
- `document.querySelectorAll(selector)`: CSS 선택자를 사용하여 모든 일치 요소의 **NodeList**를 반환합니다.
- `document.createElement(tagName)`: 새 HTML 요소를 생성합니다.
- `document.body`: `<body>` 요소를 반환합니다.

예제:
``` js
const title = document.getElementById("title");
console.log(title.textContent);
```

---

#### 2. **Element 객체**
DOM의 각 HTML 요소는 **Element** 객체로 표현됩니다.

**주요 속성 및 메서드:**

- `.innerHTML`: 요소의 HTML 콘텐츠를 읽거나 수정합니다.
- `.textContent`: 요소의 텍스트 콘텐츠를 읽거나 수정합니다.
- `.classList`: 요소의 클래스 이름 목록을 조작합니다.
  - `classList.add()`, `classList.remove()`, `classList.toggle()` 메서드로 클래스 추가/제거 가능.
- `.style`: 인라인 스타일을 설정합니다.
- `.appendChild(node)`: 자식 노드를 추가합니다.
- `.removeChild(node)`: 자식 노드를 제거합니다.

예제:
``` js
// 버튼을 클릭했을 때 텍스트를 변경
document.querySelector("button").addEventListener("click", function() {
    const p = document.querySelector("p");
    p.textContent = "버튼이 클릭되었습니다!";
});
```

---

#### 3. **Node 객체**
DOM의 모든 객체(Element, Text 등)는 **Node** 객체를 상속받습니다.

**주요 속성 및 메서드:**

- `.parentNode`: 부모 노드를 반환합니다.
- `.childNodes`: 자식 노드의 NodeList를 반환합니다.
- `.firstChild`, `.lastChild`: 첫 번째 및 마지막 자식 노드를 반환합니다.
- `.nextSibling`, `.previousSibling`: 형제 노드를 반환합니다.

---

### DOM 이벤트

JavaScript로 DOM을 조작할 때 이벤트는 중요한 역할을 합니다. 

- **addEventListener**: 특정 이벤트가 발생할 때 실행할 코드를 지정합니다.

**주요 이벤트:**

- `click`: 요소 클릭.
- `input`: 입력 필드에서 입력.
- `submit`: 폼 제출.
- `mouseover`, `mouseout`: 마우스 오버/아웃.

예제:
``` js
// 이벤트 리스너 예제
document.getElementById("btn").addEventListener("click", function() {
    alert("버튼이 클릭되었습니다!");
});
```

---

### DOM 조작 예제

#### 1. 요소 추가
``` js
const newDiv = document.createElement("div");
newDiv.textContent = "새로운 요소 추가!";
document.body.appendChild(newDiv);
```

#### 2. 요소 제거
``` js
const element = document.getElementById("remove-me");
element.parentNode.removeChild(element);
```

#### 3. 클래스 조작
``` js
const box = document.querySelector(".box");
box.classList.add("active");
box.classList.remove("inactive");
box.classList.toggle("hidden");
```

#### 4. 스타일 변경
``` js
const box = document.querySelector(".box");
box.style.backgroundColor = "blue";
box.style.color = "white";
```

---

### 실습 아이디어

1. **Todo List**: 사용자가 항목을 추가/제거할 수 있는 목록.
2. **이미지 슬라이더**: 이미지와 버튼을 통해 사진 전환.
3. **모달 창**: 버튼 클릭 시 팝업 표시/숨기기.

DOM과 이벤트를 활용한 다양한 아이디어로 웹 페이지를 동적으로 만들어 보세요!
