---
title: "CSS의 글꼴 속성: font-family부터 Google Fonts까지"
excerpt: "CSS의 글꼴 속성에 대해 font-family 설정, 웹 안전 글꼴, Google Fonts 사용법 등을 알아봅니다."
categories:
  - Web
tags:
  - [CSS, Web Design, Google Fonts, Typography, Frontend Development]
permalink: /Web/font-family-guide/
toc: true
toc_sticky: true
date: 2024-11-10
last_modified_at: 2024-11-10
---

## CSS 글꼴 속성의 이해

CSS에서 텍스트의 글꼴을 설정하는 것은 웹 디자인의 중요한 요소입니다. `font-family`, `font-size`, `font-weight`와 같은 속성을 사용해 글꼴을 조정할 수 있으며, Google Fonts와 같은 외부 서비스를 통해 다양한 스타일을 쉽게 적용할 수 있습니다.

---

### 주요 글꼴 속성

CSS에서 글꼴을 설정하는 속성은 아래와 같습니다:

| 속성            | 설명                                                                 |
|----------------|----------------------------------------------------------------------|
| `font-family`   | 텍스트에 적용할 글꼴을 지정합니다. 우선순위에 따라 여러 글꼴을 설정할 수 있습니다. |
| `font-style`    | 텍스트의 스타일(보통, 이탤릭 등)을 지정합니다.                              |
| `font-weight`   | 텍스트의 두께(굵기)를 설정합니다. 숫자(100~900) 또는 키워드(예: `bold`)를 사용합니다. |
| `font-size`     | 텍스트의 크기를 설정합니다. 단위(px, em, rem, %, vw 등)를 사용할 수 있습니다. |
| `line-height`   | 텍스트 줄 간격을 설정합니다. 숫자, %, 또는 단위를 사용할 수 있습니다.               |
| `letter-spacing`| 텍스트 글자 간격을 설정합니다.                                          |
| `word-spacing`  | 단어 간 간격을 설정합니다.                                              |

---

### `font-family`의 이해와 사용법

#### 기본 구조

```css
p {
  font-family: "Roboto", Arial, sans-serif;
}
```

- **첫 번째 글꼴**: `"Roboto"` – 우선적으로 적용할 글꼴.
- **백업 글꼴**: `Arial` – 첫 번째 글꼴이 없을 경우 사용.
- **글꼴 패밀리**: `sans-serif` – 모든 글꼴이 없을 때 적용.

#### 글꼴 패밀리 종류
- **Serif**: 글자 끝에 장식이 있는 글꼴. 예: Times New Roman.
- **Sans-serif**: 장식이 없는 글꼴. 예: Arial, Roboto.
- **Monospace**: 글자의 너비가 일정한 글꼴. 예: Courier New.
- **Cursive**: 필기체 글꼴. 예: Comic Sans MS.
- **Fantasy**: 장식적인 글꼴. 예: Papyrus.

#### 시스템 글꼴 사용
운영체제별 기본 글꼴을 사용해 일관된 사용자 경험을 제공합니다:
```css
body {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}
```

---

### Google Fonts 사용법

Google Fonts는 무료 웹 글꼴 서비스로, 사용법은 간단합니다:

1. **Google Fonts 웹사이트 접속**  
   [Google Fonts](https://fonts.google.com)에서 원하는 글꼴을 검색합니다.

2. **글꼴 선택 및 HTML 추가**  
   ```html
   <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
   ```

3. **CSS에서 글꼴 적용**
   ```css
   body {
     font-family: 'Roboto', sans-serif;
   }
   ```

---

### 웹폰트 직접 추가하기

Google Fonts를 사용하지 않고 직접 웹폰트를 추가하려면 `@font-face`를 사용합니다:

```css
@font-face {
  font-family: "CustomFont";
  src: url("customfont.woff2") format("woff2"),
       url("customfont.woff") format("woff");
  font-weight: normal;
  font-style: normal;
}

body {
  font-family: "CustomFont", Arial, sans-serif;
}
```

---

### 실습 예제

#### Google Fonts와 커스텀 설정
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Roboto', sans-serif;
      font-size: 18px;
      line-height: 1.6;
    }
    h1 {
      font-weight: 700;
      font-size: 2rem;
    }
    p {
      font-weight: 300;
    }
  </style>
</head>
<body>
  <h1>Google Fonts Example</h1>
  <p>This is a paragraph using Roboto Light (300).</p>
</body>
</html>
```

---

CSS 글꼴을 통해 웹사이트의 가독성과 미적 요소를 향상시키세요. 궁금한 점이 있다면 댓글로 알려주세요!
