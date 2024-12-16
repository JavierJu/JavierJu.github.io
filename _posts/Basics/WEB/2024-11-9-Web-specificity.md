---
title: "CSS 우선순위(Specificity) 이해하기"
excerpt: "CSS 우선순위(Specificity)에 대한 이해를 바탕으로, 어떻게 우선순위가 결정되고 충돌을 해결할 수 있는지 알아봅니다."
categories:
  - Web
tags:
  - [CSS, Web Design, Web Development]
permalink: /Web/specificity/
toc: true
toc_sticky: true
date: 2024-11-9
last_modified_at: 2024-11-9
---

## CSS 우선순위(Specificity)란?

CSS의 **우선순위(Specificity)**는 여러 규칙이 같은 요소에 적용될 때 어떤 규칙이 우선 적용될지를 결정하는 규칙입니다. 우선순위는 선택자의 유형과 구조에 따라 결정됩니다. 우선순위가 높은 규칙이 적용되며, 그 계산법을 이해하는 것은 스타일 충돌을 피하고, 보다 예측 가능한 스타일링을 만들 수 있는 중요한 기술입니다.

---

## 1. CSS 우선순위 계산 순서

CSS 우선순위는 아래와 같은 기준으로 계산됩니다:

1. **인라인 스타일**: HTML 요소에 직접 작성된 `style` 속성 (가장 높은 우선순위)

   ``` html
   <div style="color: red;">Hello</div>
   ```

2. **ID 선택자**: `#id`

   ``` css
   #header {
       color: blue;
   }
   ```

3. **클래스, 속성, 의사 클래스 선택자**: `.class`, `[attribute]`, `:hover`, `:nth-child()` 등

   ``` css
   .content {
       color: green;
   }
   ```

4. **태그 선택자**: `div`, `h1`, `p`, 등

   ``` css
   div {
       color: black;
   }
   ```

5. **와일드카드, 상위 선택자, 유산된 스타일**: `*`, `div p`, 부모 요소에서 상속된 스타일 등

   ``` css
   * {
       color: gray;
   }
   ```

---

## 2. 우선순위 점수 계산

CSS 선택자의 점수는 각 선택자 유형에 따라 계산됩니다. 점수는 4개의 부분으로 나뉘며, 각 부분의 우선순위가 합산되어 최종 우선순위가 결정됩니다:

- **인라인 스타일**: `1000`
- **ID 선택자**: `0100` (100점)
- **클래스, 속성, 의사 클래스 선택자**: `0010` (10점)
- **태그 선택자, 의사 요소 선택자**: `0001` (1점)

예를 들어:
- `#header` → `0100` (ID 선택자)
- `.content` → `0010` (클래스 선택자)
- `div` → `0001` (태그 선택자)
- `div.content:hover` → `0011` (`div` 태그 + `.content` 클래스 + `:hover` 의사 클래스)

우선순위는 점수를 비교하여 결정됩니다. 점수가 높은 규칙이 우선합니다.

---

## 3. 중요성: `!important`

`!important`가 사용된 규칙은 다른 규칙보다 항상 우선적으로 적용됩니다. 이 경우, 우선순위 점수 계산은 무시됩니다.

``` css
p {
    color: blue !important;
}
```

하지만 `!important`가 여러 번 사용된 경우, **우선순위 계산 순서**가 적용됩니다. 이 점을 고려하여 사용해야 합니다.

---

## 4. 상속과 우선순위

CSS 속성 중 일부는 부모 요소로부터 상속됩니다(예: `color`, `font-family`). 하지만 상속된 스타일은 **명시적으로 지정된 스타일보다 우선순위가 낮습니다**.

``` css
body {
    color: red; /* 상속 */
}

p {
    color: blue; /* p 요소는 이 스타일을 우선 사용 */
}
```

---

## 5. 우선순위 정리

1. **`!important`**는 항상 가장 우선합니다.
2. **인라인 스타일**은 그 다음으로 높은 우선순위를 가집니다.
3. 선택자 유형에 따라 우선순위 점수를 계산합니다:
   - ID > 클래스, 속성, 의사 클래스 > 태그
4. 동일한 점수라면 **가장 나중에 선언된** 규칙이 적용됩니다(소스 코드 순서 기준).

---

## 6. 실제 사례

아래 예시로 우선순위를 확인해봅시다:

``` html
<!DOCTYPE html>
<html>
<head>
    <style>
        div { color: black; }          /* 우선순위 0001 */
        .box { color: green; }        /* 우선순위 0010 */
        #unique { color: blue; }      /* 우선순위 0100 */
        div.box { color: yellow; }    /* 우선순위 0011 */
        #unique.box { color: red; }   /* 우선순위 0110 */
    </style>
</head>
<body>
    <div id="unique" class="box">Hello</div>
</body>
</html>
```

**결과**: 텍스트 색상은 **red**입니다.  
왜냐하면 `#unique.box`의 우선순위(`0110`)가 가장 높기 때문입니다.

---

CSS의 우선순위를 이해하면, 스타일 충돌을 줄이고 원하는 디자인을 보다 쉽게 적용할 수 있습니다. 이를 통해 웹 페이지의 시각적인 완성도를 높일 수 있습니다!
