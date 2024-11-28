---
title: "JavaScript에서 innerText, innerHTML, textContent의 차이와 활용법"
excerpt: "innerText, innerHTML, textContent의 구체적인 차이와 용도에 대해 알아보고, 각 속성의 사용 예제를 제공합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, HTML, DOM, Web Development]
permalink: /JavaScript/innerText-innerHTML-textContent/
toc: true
toc_sticky: true
date: 2024-11-28
last_modified_at: 2024-11-28
---

## 개요
JavaScript에서 DOM 요소의 내용을 읽거나 변경할 때 주로 사용하는 세 가지 속성 `innerText`, `innerHTML`, `textContent`는 각각 다른 특성과 용도를 가지고 있습니다. 이 글에서는 이들 속성의 차이점과 활용 사례를 상세히 설명합니다.

---

## 1. `innerText`
### 설명
- 요소의 "텍스트"를 나타냅니다.
- CSS에 의해 숨겨진 텍스트는 무시합니다.
- 렌더링된 텍스트만 가져오거나 설정합니다.

### 특징
- HTML 태그를 제외한 텍스트만 반환.
- 스타일에 의존하여 숨겨진 텍스트는 무시.

### 예제
``` js
<div id="example" style="display: none;">Hello <b>World</b></div>
<script>
  const element = document.getElementById("example");
  console.log(element.innerText); // 빈 문자열 출력 (display: none 때문)
  element.style.display = "block";
  console.log(element.innerText); // "Hello World"
</script>
```

### 용도
- 사용자에게 보이는 텍스트만 조작해야 할 때.
- 화면의 렌더링된 텍스트를 기준으로 작업할 때.

---

## 2. `innerHTML`
### 설명
- 요소의 "HTML 콘텐츠"를 나타냅니다.
- HTML 태그를 포함하여 콘텐츠를 가져오거나 설정할 수 있습니다.

### 특징
- HTML 태그를 포함하여 값을 읽거나 변경 가능.
- 설정 시 HTML 파싱이 이루어짐.

### 예제
``` js
<div id="example">Hello <b>World</b></div>
<script>
  const element = document.getElementById("example");
  console.log(element.innerHTML); // "Hello <b>World</b>"
  element.innerHTML = "<i>Goodbye</i> <span>World</span>";
  console.log(element.innerHTML); // "<i>Goodbye</i> <span>World</span>"
</script>
```

### 용도
- HTML 구조를 포함한 콘텐츠를 동적으로 변경해야 할 때.
- 사용자 입력을 처리할 때는 XSS 공격 방지 필요.

---

## 3. `textContent`
### 설명
- 요소의 "텍스트 콘텐츠"를 나타냅니다.
- 태그를 제외하고 모든 텍스트를 가져오거나 설정합니다.

### 특징
- HTML 태그는 무시.
- CSS에 의해 숨겨진 텍스트도 가져옴.

### 예제
``` js
<div id="example" style="display: none;">Hello <b>World</b></div>
<script>
  const element = document.getElementById("example");
  console.log(element.textContent); // "Hello World" (숨겨진 텍스트도 포함)
  element.textContent = "New Text";
  console.log(element.textContent); // "New Text"
</script>
```

### 용도
- 태그를 무시하고 순수한 텍스트만 필요할 때.
- 숨겨진 텍스트를 포함해야 할 때.

---

## 4. 비교 요약

| 속성           | HTML 태그 포함 | 숨겨진 텍스트 | 동작 방식                    | 주요 용도                           |
|----------------|----------------|----------------|----------------------------|-----------------------------------|
| `innerText`    | ❌            | ❌            | 렌더링된 텍스트 반환         | 사용자가 볼 수 있는 텍스트 처리      |
| `innerHTML`    | ✅            | ❌            | HTML 태그 포함, 파싱         | HTML 구조 변경                     |
| `textContent`  | ❌            | ✅            | 순수 텍스트 반환             | 태그 없이 텍스트만 필요할 때        |

---

## 5. 예제 비교

다음은 세 가지 속성을 사용하여 같은 요소를 처리하는 예제입니다.

``` js
<div id="example" style="display: none;">
  Hello <b>World</b>
</div>
<script>
  const element = document.getElementById("example");
  
  console.log("innerText:", element.innerText); // ""
  console.log("innerHTML:", element.innerHTML); // "Hello <b>World</b>"
  console.log("textContent:", element.textContent); // "Hello World"

  // 각각의 값을 변경
  element.innerText = "New <i>Text</i>";
  console.log(element.innerHTML); // "New &lt;i&gt;Text&lt;/i&gt;"

  element.innerHTML = "<p>New <i>HTML</i></p>";
  console.log(element.textContent); // "New HTML"
</script>
```

---

## 결론
`innerText`, `innerHTML`, `textContent`는 각기 다른 특성과 용도를 가집니다. HTML 태그를 조작해야 할 때는 `innerHTML`을, 순수 텍스트를 다룰 때는 `textContent`를, 사용자에게 보이는 텍스트를 조작할 때는 `innerText`를 사용하세요. 프로젝트의 요구사항에 맞는 적절한 속
