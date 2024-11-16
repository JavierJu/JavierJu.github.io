---
title: "CSS 의사 클래스: hover와 focus의 차이점"
excerpt: "hover와 focus의 의사 클래스를 사용하여 웹 요소의 상호작용 상태를 정의하고 스타일링하는 방법에 대해 알아봅니다."
categories:
  - WEB
tags:
  - [CSS, Frontend, Web Design, User Interaction, Pseudo-classes]
permalink: /WEB/css-hover-focus-difference/
toc: true
toc_sticky: true
date: 2024-11-17
last_modified_at: 2024-11-17
---

CSS에서 웹 요소의 사용자 상호작용 상태를 정의하는 데 사용되는 두 가지 주요 의사 클래스는 `hover`와 `focus`입니다. 이 글에서는 각 클래스의 정의와 사용법, 그리고 이 둘의 차이점에 대해 설명합니다.

## 1. `hover`란?
`hover`는 마우스 포인터가 요소 위에 있을 때 활성화되는 의사 클래스입니다. 사용자가 요소 위로 마우스를 올렸을 때 시각적인 변화를 주기 위해 주로 사용됩니다.

### 예제:
```css
button:hover {
  background-color: lightblue;
}
```
위의 코드는 사용자가 버튼에 마우스를 올렸을 때 버튼의 배경색을 연한 파란색으로 변경합니다.

## 2. `focus`란?
`focus`는 요소가 키보드 입력을 받을 수 있는 상태일 때 적용되는 의사 클래스입니다. 텍스트 입력 필드, 버튼과 같은 상호작용 가능한 요소에서 자주 사용됩니다. 탭 키를 사용해 포커스를 이동하거나 마우스로 클릭하여 요소를 활성화할 때 `focus` 상태가 적용됩니다.

### 예제:
```css
input:focus {
  border-color: blue;
}
```
이 코드는 입력 필드가 포커스 상태일 때 테두리 색을 파란색으로 변경합니다.

## 3. `hover`와 `focus`의 차이점
- **`hover`**는 마우스 포인터가 요소 위에 있을 때 작동하며, 포커스를 유지하지 않습니다.
- **`focus`**는 키보드로 요소를 선택하거나 클릭으로 활성화할 때 적용되며, 그 상태를 유지할 수 있습니다.

## 4. 함께 사용하기
`hover`와 `focus`를 함께 적용할 수도 있습니다. 이를 통해 요소가 포커스 상태나 마우스 오버 상태일 때 동일한 스타일을 적용할 수 있습니다.

### 예제:
```css
button:hover, button:focus {
  background-color: lightgreen;
}
```
이 코드는 버튼에 마우스를 올리거나 키보드로 포커스를 이동했을 때 배경색을 연한 녹색으로 변경합니다.

## 결론
`hover`와 `focus`는 사용자가 웹 페이지와 상호작용할 때 시각적 피드백을 제공하는 중요한 CSS 의사 클래스입니다. 두 클래스를 적절히 사용하여 더 나은 사용자 경험을 제공합니다.
