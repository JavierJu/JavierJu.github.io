---
title: "CSS 박스 모델과 디스플레이 속성에 대한 이해"
excerpt: "CSS의 박스 모델과 디스플레이 속성에 대해 자세히 알아보고, 웹 페이지 레이아웃 설계 시 활용하는 방법을 소개합니다."
categories:
  - Web
tags:
  - [CSS, Web Design, Box Model, Display Property]
permalink: /Web/box-model-display-properties/
toc: true
toc_sticky: true
date: 2024-11-9
last_modified_at: 2024-11-9
---

## CSS 박스 모델 (Box Model)

CSS의 **박스 모델**은 웹 페이지에서 요소가 차지하는 공간을 계산하는 기본적인 원리입니다. 박스 모델은 네 가지 영역으로 구성됩니다:

1. **Content (내용)**  
   - 요소의 텍스트나 이미지가 들어가는 영역입니다.  
   - `width`와 `height`로 크기를 조절합니다.

2. **Padding (패딩)**  
   - 콘텐츠와 테두리(border) 사이의 간격입니다.  
   - `padding` 속성을 사용하여 조정하며, 배경색이 포함됩니다.

3. **Border (테두리)**  
   - 콘텐츠와 패딩 주위에 둘러싸인 선입니다.  
   - `border-width`, `border-style`, `border-color`로 스타일을 지정합니다.

4. **Margin (마진)**  
   - 요소와 다른 요소 사이의 외부 간격입니다.  
   - `margin` 속성을 사용해 설정합니다.  

### 박스 모델 예시
```css
.box {
  width: 200px; /* 콘텐츠 영역의 너비 */
  height: 100px; /* 콘텐츠 영역의 높이 */
  padding: 10px; /* 콘텐츠와 테두리 사이의 간격 */
  border: 5px solid black; /* 테두리 */
  margin: 20px; /* 요소 외부 여백 */
}
```

### 박스 모델의 계산 방식

총 요소의 크기 =  
`content width + padding + border + margin`

### Box-Sizing 속성

`box-sizing` 속성을 사용하면 박스 모델의 계산 방식을 변경할 수 있습니다:
- `content-box` (기본값): `width`와 `height`는 **콘텐츠 크기**만 의미합니다.
- `border-box`: `width`와 `height`가 **패딩과 테두리를 포함**한 전체 크기를 의미합니다.

```css
.box {
  box-sizing: border-box; /* 테두리와 패딩 포함한 크기를 width, height로 계산 */
}
```

---

## 디스플레이(Display) 속성

`display`는 HTML 요소의 배치와 동작 방식을 정의하는 중요한 속성입니다.

### 주요 값

1. **`block`**  
   - 요소가 **새 줄**에서 시작하며, 기본적으로 **전체 너비**를 차지합니다.  
   - 예: `<div>`, `<p>`, `<h1>`

2. **`inline`**  
   - 요소가 **같은 줄**에 배치되며, **콘텐츠 크기**만큼만 공간을 차지합니다.  
   - 너비와 높이를 설정할 수 없습니다.  
   - 예: `<span>`, `<a>`

3. **`inline-block`**  
   - **inline**처럼 같은 줄에 배치되지만, **block**처럼 너비와 높이를 설정할 수 있습니다.  

4. **`flex`**  
   - **유연한 컨테이너**를 생성하여 자식 요소들을 배치합니다.  
   - 자식 요소 간의 정렬, 간격 등을 쉽게 조정할 수 있습니다.  

5. **`grid`**  
   - 2차원 레이아웃을 설계할 수 있도록 해줍니다.  
   - 행(row)과 열(column) 기반의 레이아웃 관리에 적합합니다.

6. **`none`**  
   - 요소를 화면에서 완전히 **제거**합니다. (DOM에는 남아있음)

---

### 디스플레이 속성 예시

```css
.block {
  display: block;
  background-color: lightblue;
}

.inline {
  display: inline;
  background-color: lightcoral;
}

.flex {
  display: flex;
  gap: 10px; /* 플렉스 아이템 간의 간격 */
}
```

---

## 박스 모델과 디스플레이 조합의 중요성

박스 모델과 디스플레이 속성을 이해하면 요소의 크기와 위치를 더 효과적으로 관리할 수 있습니다. 예를 들어:
- `flex`와 `grid`를 사용하면 복잡한 레이아웃을 쉽게 조정할 수 있습니다.
- `inline-block`은 버튼 같은 요소를 정렬할 때 유용합니다.

웹 페이지에서 요소를 배치하고 크기를 설정하는 방법을 숙지하면 레이아웃을 더 직관적이고 효율적으로 설계할 수 있습니다.
