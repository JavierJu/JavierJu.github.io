---
title: "CSS 배경 속성 완벽 가이드: 색상, 그라데이션, 이미지 설정"
excerpt: "CSS 배경 속성을 활용해 웹 요소의 스타일링을 더욱 풍부하게 만드는 방법을 알아봅니다. 배경 색상, 그라데이션, 이미지 배치 및 반복 설정 등을 다룹니다."
categories:
  - Web
tags:
  - [CSS, Web Design, Front-End, Styling, Background]
permalink: /Web/background-guide/
toc: true
toc_sticky: true
date: 2024-11-10
last_modified_at: 2024-11-10
---

CSS에서 배경(`background`) 속성은 웹 요소의 스타일링에서 중요한 역할을 합니다. 이번 글에서는 배경 색상, 그라데이션, 이미지 설정 등 다양한 옵션과 예제를 통해 배경 속성을 완벽히 이해할 수 있도록 설명합니다.

## 1. 배경 색상 (Background Color)

배경 색상은 `background-color` 속성을 사용하여 설정할 수 있습니다.

- **값 옵션**:
  - **색상 이름**: `red`, `blue`, `green` 등
  - **16진수 값**: `#ff0000` (빨강)
  - **RGB**: `rgb(255, 0, 0)` (빨강)
  - **RGBA**: `rgba(255, 0, 0, 0.5)` (50% 투명 빨강)
  - **HSL**: `hsl(0, 100%, 50%)` (빨강)
  - **HSLA**: `hsla(0, 100%, 50%, 0.5)` (50% 투명 빨강)

**예제**:
``` css
div {
  background-color: #3498db; /* 파란색 */
}
```

---

## 2. 배경 그라데이션

배경 색상을 부드럽게 전환하는 효과를 줄 때 `linear-gradient`와 `radial-gradient`를 사용할 수 있습니다.

### (1) 선형 그라데이션 (Linear Gradient)

- **속성**: `background-image: linear-gradient();`
- **옵션**:
  - **각도**: `to right`, `90deg` 등
  - **색상 단계**: 여러 색상 지정 가능

**예제**:
``` css
div {
  background-image: linear-gradient(to right, #ff7e5f, #feb47b);
}
```
→ 왼쪽에서 오른쪽으로 주황색에서 분홍색으로 전환.

### (2) 원형 그라데이션 (Radial Gradient)

- **속성**: `background-image: radial-gradient();`
- **옵션**:
  - **형태**: `circle`, `ellipse`
  - **위치**: `center`, `top left` 등

**예제**:
``` css
div {
  background-image: radial-gradient(circle, #ff7e5f, #feb47b);
}
```

---

## 3. 배경 이미지

`background-image` 속성을 사용하여 요소에 이미지를 추가할 수 있습니다.

- **값 옵션**:
  - `url('image.jpg')`: 이미지 파일 경로 지정
  - `none`: 배경 이미지를 사용하지 않음

**예제**:
``` css
div {
  background-image: url('background.jpg');
}
```

---

## 4. 배경 이미지 배치 옵션

### (1) 반복 설정 (`background-repeat`)
이미지가 요소를 채울 때 반복 여부를 설정합니다.

- **옵션**:
  - `repeat`: 기본값, 가로/세로 모두 반복
  - `repeat-x`: 가로 방향만 반복
  - `repeat-y`: 세로 방향만 반복
  - `no-repeat`: 반복하지 않음

**예제**:
``` css
div {
  background-image: url('pattern.png');
  background-repeat: no-repeat;
}
```

### (2) 이미지 크기 (`background-size`)
배경 이미지의 크기를 설정합니다.

- **옵션**:
  - `auto`: 원본 크기 유지
  - `cover`: 요소를 완전히 덮도록 조정
  - `contain`: 이미지가 요소 안에 완전히 들어가도록 조정
  - 특정 크기: `100px 50px`

**예제**:
``` css
div {
  background-image: url('background.jpg');
  background-size: cover;
}
```

### (3) 이미지 위치 (`background-position`)
배경 이미지의 시작 위치를 지정합니다.

- **옵션**:
  - 키워드: `center`, `top`, `bottom`, `left`, `right`
  - 좌표: `50% 50%`, `10px 20px`

**예제**:
``` css
div {
  background-image: url('background.jpg');
  background-position: center;
}
```

### (4) 고정 또는 스크롤 (`background-attachment`)
배경 이미지가 스크롤에 따라 움직이는지 설정합니다.

- **옵션**:
  - `scroll`: 기본값, 콘텐츠와 함께 움직임
  - `fixed`: 배경 고정
  - `local`: 스크롤 요소에 맞춰 움직임

**예제**:
``` css
div {
  background-image: url('background.jpg');
  background-attachment: fixed;
}
```

---

## 5. 배경 복합 설정

여러 배경 속성을 한 번에 설정하려면 `background` 속성을 사용하세요.

**구조**:
`background: color image position/size repeat attachment;`

**예제**:
``` css
div {
  background: #ffffff url('pattern.png') no-repeat center/cover fixed;
}
```
→ 흰 배경, 이미지가 중앙 고정, 크기는 요소를 덮도록 조정.

---

## 전체 예제

아래는 다양한 배경 속성을 조합한 예제입니다.

**HTML & CSS 코드**:
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CSS Background Demo</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
    }

    .example {
      width: 100%;
      height: 300px;
      background: linear-gradient(to right, #ff7e5f, #feb47b), 
                  url('background.jpg') no-repeat center/cover;
      background-attachment: fixed;
    }
  </style>
</head>
<body>
  <div class="example"></div>
</body>
</html>
```
이 코드를 활용해 다양한 배경 효과를 실험해보세요! 😊
