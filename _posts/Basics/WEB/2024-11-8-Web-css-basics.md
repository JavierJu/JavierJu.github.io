---
title: "CSS 기초 - 색상, 배경색, 텍스트 속성 등"
excerpt: "CSS의 기본 속성들을 정리하여 웹 페이지 스타일을 쉽게 적용하는 방법을 알아봅니다."
categories:
  - Web
tags:
  - [CSS, Web Design, HTML, Styling]
permalink: /Web/css-basics/
toc: true
toc_sticky: true
date: 2024-11-8
last_modified_at: 2024-11-8
---

# CSS 기초 - 색상, 배경색, 텍스트 속성 등

CSS (Cascading Style Sheets)는 웹 페이지의 스타일을 정의하는 언어입니다. HTML 요소에 색상, 글꼴, 간격 등 다양한 스타일을 적용하는 데 사용됩니다. 이 글에서는 CSS의 기초적인 사용법과 자주 사용하는 속성들에 대해 알아보겠습니다.

## 1. 색상 (Color)

CSS에서 색상을 지정하는 방법은 여러 가지가 있습니다:

- **색상 이름**: `red`, `blue`, `green` 등.
  ```css
  p {
    color: red;
  }
  ```
- **16진수 코드**: `#RRGGBB` 형식으로, 각 두 자리는 빨간색, 초록색, 파란색의 강도를 나타냅니다.
  ```css
  p {
    color: #ff6347;  /* Tomato 색상 */
  }
  ```
- **RGB 값**: `rgb(red, green, blue)` 형식으로, 각 색상의 0부터 255까지의 값을 사용합니다.
  ```css
  p {
    color: rgb(255, 99, 71);  /* Tomato 색상 */
  }
  ```
- **RGBA 값**: RGB에 투명도(Alpha)를 추가한 값으로, 투명도를 0부터 1까지 설정할 수 있습니다.
  ```css
  p {
    color: rgba(255, 99, 71, 0.5);  /* 반투명 Tomato 색상 */
  }
  ```

## 2. 배경색 (Background Color)

배경색을 설정할 때는 `background-color` 속성을 사용합니다.
```css
div {
  background-color: lightblue;
}
```

## 3. 세미콜론 (Semicolon)

CSS에서 각 속성은 **세미콜론(;)**으로 구분됩니다. 여러 속성을 한 번에 설정할 때 세미콜론을 사용해 각 속성을 끝내야 합니다. 마지막 속성에도 세미콜론을 넣는 것이 좋습니다. 
```css
h1 {
  font-size: 24px;
  color: black;
  background-color: lightgray;
}
```

## 4. 일반 텍스트 속성

일반 텍스트를 스타일링하는 여러 속성이 있습니다.

- **글꼴 크기 (Font Size)**: 텍스트의 크기를 설정하는 속성입니다.
  ```css
  p {
    font-size: 16px;
  }
  ```

- **글꼴 (Font Family)**: 텍스트에 사용할 글꼴을 지정하는 속성입니다.
  ```css
  p {
    font-family: Arial, sans-serif;
  }
  ```

- **글꼴 두께 (Font Weight)**: 텍스트의 두께를 설정합니다.
  ```css
  p {
    font-weight: bold;
  }
  ```

- **글자 스타일 (Font Style)**: 텍스트를 기울이거나 설정합니다.
  ```css
  p {
    font-style: italic;
  }
  ```

- **텍스트 정렬 (Text Align)**: 텍스트의 정렬 방식을 설정합니다.
  ```css
  p {
    text-align: center;
  }
  ```

## 5. 기타 텍스트 관련 속성

- **줄 높이 (Line Height)**: 텍스트 줄 간격을 설정합니다.
  ```css
  p {
    line-height: 1.5;
  }
  ```

- **텍스트 색상 (Text Color)**: 텍스트 색을 지정합니다.
  ```css
  p {
    color: #333333;
  }
  ```

## 6. 댓글 (Comments)

CSS에서는 `/*` 와 `*/`로 감싸서 주석을 작성할 수 있습니다. 주석은 브라우저에서 무시되고, 코드에 대한 설명을 추가하는 데 유용합니다.
```css
/* 이건 주석입니다 */
p {
  font-size: 16px;  /* 텍스트 크기 설정 */
}
```

## 7. 클래스와 아이디 선택자

- **클래스 선택자**: HTML 요소에 `class` 속성을 사용하여 스타일을 적용합니다. 클래스는 `.`으로 시작합니다.
  ```css
  .button {
    background-color: blue;
    color: white;
  }
  ```
- **아이디 선택자**: HTML 요소에 `id` 속성을 사용하여 스타일을 적용합니다. 아이디는 `#`으로 시작합니다.
  ```css
  #header {
    font-size: 32px;
  }
  ```

## 8. 배치 속성

- **여백 (Margin)**: 요소의 외부 여백을 설정합니다.
  ```css
  div {
    margin: 20px;
  }
  ```
- **안쪽 여백 (Padding)**: 요소의 내부 여백을 설정합니다.
  ```css
  div {
    padding: 15px;
  }
  ```

## 9. 박스 모델

CSS의 박스 모델은 요소의 너비, 높이, 여백(margin), 테두리(border), 안쪽 여백(padding) 등으로 구성됩니다.

```css
div {
  width: 100px;
  height: 100px;
  margin: 20px;
  padding: 15px;
  border: 2px solid black;
}
```

## 10. CSS 선택자 (Selectors)

- **태그 선택자**: HTML 태그 이름으로 스타일을 적용합니다.
  ```css
  h1 {
    color: green;
  }
  ```

- **자식 선택자**: 부모 요소의 자식 요소에 스타일을 적용합니다.
  ```css
  div > p {
    color: red;
  }
  ```

이렇게 CSS의 기본 속성들을 통해 웹 페이지의 디자인을 조정할 수 있습니다. 필요에 따라 다양한 속성을 조합하여 더 복잡한 스타일을 만들 수 있습니다.
