---
title: "JavaScript로 HTML 스타일 변경: 다양한 방법 정리"
excerpt: "JavaScript를 활용하여 HTML 요소의 스타일을 동적으로 변경하는 다양한 방법을 비교하고, 각각의 장단점과 활용 사례를 살펴봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, HTML, DOM, CSS]
permalink: /JavaScript/html-style-change/
toc: true
toc_sticky: true
date: 2024-11-29
last_modified_at: 2024-11-29
---

HTML 요소의 스타일을 동적으로 변경하는 것은 사용자 경험을 향상시키고, 동적인 웹 애플리케이션을 만들기 위해 필수적인 작업입니다. JavaScript에서 스타일을 변경하는 다양한 방법과 각각의 장단점을 정리했습니다.

---

## 1. `style` 속성 직접 조작

HTML 요소의 `style` 속성은 인라인 스타일을 직접 변경할 때 사용됩니다.

### 특징
- **즉각적이고 간단**: 개별 속성을 쉽게 변경 가능.
- **제한적**: 이미 적용된 클래스나 외부 스타일시트를 대체하지 않음.

### 예제
``` js
const element = document.getElementById('box');

// 스타일 설정
element.style.color = 'red';
element.style.backgroundColor = 'yellow';

// 기존 스타일 읽기
console.log(element.style.color); // "red"

// 스타일 제거
element.style.color = '';
```
### 단점
- **유지보수 어려움**: 여러 스타일을 한 번에 적용하거나 제거하기 어려움.
- **CSS 클래스 재사용 불가**: 스타일 재사용이 어려움.

---

## 2. CSS 클래스 변경

CSS 클래스는 요소의 스타일을 그룹화하고 재사용할 수 있도록 만들어줍니다. JavaScript로 클래스 변경은 `className`과 `classList`를 통해 가능합니다.

### 2.1 `className` 사용

`className`은 요소의 클래스 전체를 읽거나 설정합니다.

#### 예제
``` js
const element = document.getElementById('box');

// 클래스 설정
element.className = 'highlight';

// 클래스 읽기
console.log(element.className); // "highlight"

// 기존 클래스 유지하며 추가
element.className += ' active';

// 클래스 초기화
element.className = '';
```
#### 단점
- 기존 클래스가 덮어써질 위험.
- 문자열 처리 필요.

### 2.2 `classList` 사용

`classList`는 클래스 조작을 위한 현대적인 API입니다.

#### 주요 메서드
- `add(className)`: 클래스 추가.
- `remove(className)`: 클래스 제거.
- `toggle(className)`: 클래스 추가/제거 토글.
- `contains(className)`: 클래스 포함 여부 확인.

#### 예제
``` js
const element = document.getElementById('box');

// 클래스 추가
element.classList.add('highlight');

// 클래스 제거
element.classList.remove('active');

// 클래스 토글
element.classList.toggle('hidden');

// 클래스 포함 여부 확인
if (element.classList.contains('hidden')) {
  console.log('hidden 클래스가 있습니다.');
}
```
#### 장점
- 기존 클래스에 영향을 주지 않음.
- 가독성과 유지보수성 우수.

---

## 3. CSS 변수 변경

CSS 변수는 스타일을 동적으로 변경할 때 강력한 도구입니다. `style.setProperty`를 사용하여 변수 값을 변경할 수 있습니다.

### 예제
``` css
:root {
  --main-color: blue;
}
#box {
  color: var(--main-color);
}
```
JavaScript:
``` js
const root = document.documentElement;

// CSS 변수 변경
root.style.setProperty('--main-color', 'red');
```
#### 장점
- 전역 스타일에 영향.
- 여러 요소에 쉽게 적용.

---

## 4. `setAttribute`와 `getAttribute`

`setAttribute`와 `getAttribute`를 사용하면 요소의 속성을 직접 조작할 수 있습니다. 클래스 외에도 데이터 속성이나 ID를 변경할 때 유용합니다.

#### 예제
``` js
const element = document.getElementById('box');

// 클래스 설정
element.setAttribute('class', 'newClass');

// 클래스 읽기
const currentClass = element.getAttribute('class');
console.log(currentClass); // "newClass"
```

---

## 5. CSSOM 활용

CSSOM(CSS Object Model)은 JavaScript로 CSS 스타일시트를 직접 변경할 수 있게 합니다.

#### 예제
``` js
const sheet = document.styleSheets[0];

// 새로운 규칙 추가
sheet.insertRule('#box { color: green; }', sheet.cssRules.length);

// 기존 규칙 읽기
console.log(sheet.cssRules[0].cssText);
```
#### 특징
- 스타일시트 직접 조작 가능.
- 복잡한 스타일 변경 가능.

---

## 결론

### 상황별 적합한 방법 선택
- **개별 속성 변경**: `style` 속성.
- **클래스 기반 변경**: `classList`.
- **CSS 변수 활용**: 전역 스타일 변경 시.
- **속성 조작**: `setAttribute`/`getAttribute`.
- **스타일시트 변경**: CSSOM.

JavaScript를 활용한 스타일 변경은 프로젝트의 요구 사항에 맞춰 적절한 방법을 선택하는 것이 중요합니다. 효율적이고 가독성 높은 코드를 작성하여 유지보수를 용이하게 만드세요.
