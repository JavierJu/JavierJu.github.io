---
title: "Starability: CSS로 구현하는 간단하고 세련된 별점 평가 UI"
excerpt: "Starability를 활용해 JavaScript 없이 HTML과 CSS로 별점 평가 UI를 구현하는 방법과 주요 특징을 살펴봅니다."
categories:
  - Web
  - CSS
  - UI/UX
  - Open Source
  
tags:
  - [CSS, 별점 평가, UI, 오픈소스, Starability]
permalink: /web/starability-introduction/
toc: true
toc_sticky: true
date: 2025-01-07
last_modified_at: 2025-01-07
---

**Starability**는 순수 HTML과 CSS를 활용해 별점 평점 UI를 구현할 수 있는 오픈소스 라이브러리입니다. JavaScript 없이도 다양한 애니메이션과 디자인의 별점 입력을 쉽게 적용할 수 있어 간단하면서도 세련된 UI를 구현하는 데 적합합니다.

## Starability의 주요 특징

1. **CSS 기반**: JavaScript 없이 CSS만으로 작동합니다.
2. **접근성(A11Y)**: 스크린 리더와 호환되며, 접근성을 준수한 설계를 제공합니다.
3. **다양한 스타일**:
   - `starability-basic`: 기본 별점 스타일
   - `starability-grow`: 별이 커지는 애니메이션
   - `starability-growRotate`: 별이 회전하며 커지는 애니메이션
   - `starability-slot`: 슬롯머신처럼 애니메이션
   - `starability-fade`: 별이 점점 나타나는 효과
   - `starability-checkmark`: 체크박스 스타일과 조합
   - `starability-heart`: 하트 모양 스타일
4. **가벼운 파일 크기**: 라이브러리 크기가 작아 성능에 영향을 주지 않습니다.
5. **쉬운 통합**: 간단한 HTML과 CSS 클래스를 사용해 적용할 수 있습니다.

---

## 설치 및 사용 방법

### 1. 설치

#### GitHub에서 다운로드
[Starability GitHub Repository](https://github.com/LunarLogic/starability)에서 최신 CSS 파일을 다운로드합니다.

#### CDN 사용
HTML 파일에 다음과 같이 링크를 추가하여 사용할 수도 있습니다:

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/LunarLogic/starability/starability-min.css">
```

---

### 2. HTML 구조

Starability는 `<input>`과 `<label>` 요소를 활용해 별점을 표시합니다. 다음은 기본 HTML 구조입니다:

```html
<form>
  <fieldset class="starability-basic">
    <legend>Rate this:</legend>
    <input type="radio" id="rate5" name="rating" value="5">
    <label for="rate5" title="Amazing">5 stars</label>

    <input type="radio" id="rate4" name="rating" value="4">
    <label for="rate4" title="Good">4 stars</label>

    <input type="radio" id="rate3" name="rating" value="3">
    <label for="rate3" title="Average">3 stars</label>

    <input type="radio" id="rate2" name="rating" value="2">
    <label for="rate2" title="Not good">2 stars</label>

    <input type="radio" id="rate1" name="rating" value="1">
    <label for="rate1" title="Terrible">1 star</label>
  </fieldset>
</form>
```

---

### 3. 스타일 변경
`fieldset`의 클래스 이름을 변경하면 다양한 스타일을 적용할 수 있습니다. 예를 들어:

#### Grow 스타일 (성장 애니메이션)

```html
<form>
  <fieldset class="starability-grow">
    <legend>Rate this:</legend>
    <input type="radio" id="rate5" name="rating" value="5">
    <label for="rate5" title="Amazing">5 stars</label>

    <input type="radio" id="rate4" name="rating" value="4">
    <label for="rate4" title="Good">4 stars</label>

    <input type="radio" id="rate3" name="rating" value="3">
    <label for="rate3" title="Average">3 stars</label>

    <input type="radio" id="rate2" name="rating" value="2">
    <label for="rate2" title="Not good">2 stars</label>

    <input type="radio" id="rate1" name="rating" value="1">
    <label for="rate1" title="Terrible">1 star</label>
  </fieldset>
</form>
```

#### 하트 모양 스타일

```html
<form>
  <fieldset class="starability-heart">
    <legend>Rate this:</legend>
    <input type="radio" id="rate5" name="rating" value="5">
    <label for="rate5" title="Amazing">5 hearts</label>

    <input type="radio" id="rate4" name="rating" value="4">
    <label for="rate4" title="Good">4 hearts</label>

    <input type="radio" id="rate3" name="rating" value="3">
    <label for="rate3" title="Average">3 hearts</label>

    <input type="radio" id="rate2" name="rating" value="2">
    <label for="rate2" title="Not good">2 hearts</label>

    <input type="radio" id="rate1" name="rating" value="1">
    <label for="rate1" title="Terrible">1 heart</label>
  </fieldset>
</form>
```

---

### 4. 별점 결과 표시

Starability는 별점 입력뿐 아니라 결과를 표시할 수도 있습니다. 다음 코드를 사용해 별점 결과를 표시할 수 있습니다:

```html
<h3>Rated element name</h3>
<p class="starability-result" data-rating="3">
  Rated: 3 stars
</p>
```

`data-rating` 속성의 값을 변경하여 사용자가 평가한 별점을 표시합니다.

---

## Starability 커스터마이징

Starability는 CSS로 작성되어 있어 별 모양, 크기, 색상을 직접 수정할 수 있습니다.

### 별 크기 변경
```css
.starability-basic label {
  font-size: 2rem; /* 별 크기 조정 */
}
```

### 색상 변경
```css
.starability-basic label {
  color: gold; /* 기본 색상 */
}
```

---

## Starability의 장점
- **간단함**: 초보자도 쉽게 사용할 수 있습니다.
- **접근성**: A11Y 표준을 준수하여 누구나 사용할 수 있습니다.
- **다양성**: 여러 스타일 중 선택하여 프로젝트에 맞는 디자인을 적용할 수 있습니다.

## 참고 자료
- [Starability GitHub Repository](https://github.com/LunarLogic/starability)
- [Starability 데모 페이지](https://lunarlogic.github.io/starability/)

Starability는 직관적이고 가벼운 별점 UI를 구현하려는 프로젝트에 이상적인 솔루션입니다. 추가적인 질문이 있다면 댓글로 남겨주세요! 😊
