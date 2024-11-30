---
title: "CSS 선택자 정리"
excerpt: "CSS에서 다양한 선택자들을 유형별로 구체적으로 설명합니다."
categories:
  - Web
tags:
  - [CSS, Web Design, Frontend]
permalink: /Web/selectors/
toc: true
toc_sticky: true
date: 2024-11-9
last_modified_at: 2024-11-9
---

## 1. 기본 선택자

### 1.1. 전체 선택자 (`*`)
- HTML 문서의 모든 요소를 선택합니다.
```css
* {
  margin: 0;
  padding: 0;
}
```

### 1.2. 태그 선택자
- 특정 태그 이름을 가진 요소를 선택합니다.
```css
p {
  color: blue;
}
```

### 1.3. 클래스 선택자 (`.`)
- 특정 클래스 이름을 가진 요소를 선택합니다.
```css
.intro {
  font-size: 16px;
}
```

### 1.4. 아이디 선택자 (`#`)
- 특정 아이디를 가진 요소를 선택합니다. ID는 문서에서 고유해야 합니다.
```css
#header {
  background-color: gray;
}
```

---

## 2. 결합 선택자

### 2.1. 후손 선택자 (공백)
- 특정 요소의 모든 후손 요소를 선택합니다.
```css
div p {
  color: red; /* div 내부의 모든 p 요소 */
}
```

### 2.2. 자식 선택자 (`>`)
- 특정 요소의 직계 자식 요소만 선택합니다.
```css
ul > li {
  list-style: none;
}
```

### 2.3. 형제 선택자
- **인접 형제 선택자 (`+`)**: 특정 요소 바로 다음에 오는 형제를 선택합니다.
```css
h1 + p {
  font-size: 14px;
}
```
- **일반 형제 선택자 (`~`)**: 특정 요소 뒤에 오는 모든 형제를 선택합니다.
```css
h1 ~ p {
  color: green;
}
```

---

## 3. 그룹 선택자 (`,`)
- 여러 선택자를 그룹화하여 동일한 스타일을 적용합니다.
```css
h1, h2, h3 {
  font-weight: bold;
}
```

---

## 4. 속성 선택자

### 4.1. 기본 속성 선택자 (`[attr]`)
- 특정 속성이 있는 요소를 선택합니다.
```css
[input] {
  border: 1px solid black;
}
```

### 4.2. 속성 값 선택자
- 특정 속성값이 있는 요소를 선택합니다.
```css
[input="text"] {
  background-color: yellow;
}
```

### 4.3. 부분 일치 속성 선택자
- **시작 값 (`^=`)**: 특정 값으로 시작하는 속성을 선택합니다.
```css
a[href^="https"] {
  color: blue;
}
```
- **끝 값 (`$=`)**: 특정 값으로 끝나는 속성을 선택합니다.
```css
img[src$=".jpg"] {
  border-radius: 5px;
}
```
- **포함 값 (`*=`)**: 특정 값을 포함하는 속성을 선택합니다.
```css
a[href*="example"] {
  text-decoration: underline;
}
```

---

## 5. 의사 클래스

### 5.1. 동적 상태 의사 클래스
- **:hover**: 요소에 마우스를 올렸을 때.
- **:focus**: 요소가 포커스를 받았을 때.
- **:active**: 요소가 활성 상태일 때.

```css
button:hover {
  background-color: lightblue;
}
```

### 5.2. 구조 의사 클래스
- **:first-child**: 부모의 첫 번째 자식.
- **:last-child**: 부모의 마지막 자식.
- **:nth-child(n)**: 부모의 n번째 자식.

```css
li:first-child {
  font-weight: bold;
}

li:nth-child(2) {
  color: red;
}
```

---

## 6. 의사 요소
- 특정 요소의 일부를 선택합니다.
  - **::before**: 요소 내용 앞에 가상 콘텐츠를 추가.
  - **::after**: 요소 내용 뒤에 가상 콘텐츠를 추가.
  - **::first-line**: 첫 번째 줄.
  - **::first-letter**: 첫 번째 글자.

```css
p::before {
  content: "★ ";
  color: gold;
}
```

---

## 7. CSS 우선순위

선택자의 **특이성(specificity)**에 따라 적용되는 우선순위가 결정됩니다.
- 인라인 스타일 (`style="..."`) > ID 선택자 > 클래스 선택자 > 태그 선택자 > 전체 선택자.

### 예제: 우선순위 비교

```html
<p id="main" class="highlight">Hello</p>
```

```css 
p { color: black; }          /* 1 */ 
.highlight { color: blue; }  /* 10 */ 
#main { color: red; }        /* 100 */
```
결과: `red` (ID 선택자의 우선순위가 가장 높음)

