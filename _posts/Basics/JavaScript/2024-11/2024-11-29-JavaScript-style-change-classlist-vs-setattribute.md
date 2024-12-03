---
title: "JavaScript에서 스타일 변경: classList와 setAttribute의 차이점"
excerpt: "JavaScript로 HTML 요소의 스타일을 변경하는 다양한 방법과 classList와 setAttribute의 활용 사례를 비교합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, HTML, DOM, Web Development]
permalink: /JavaScript/style-change-classlist-vs-setattribute/
toc: true
toc_sticky: true
date: 2024-11-29
last_modified_at: 2024-11-29
---

HTML 요소의 스타일을 동적으로 변경하는 것은 JavaScript에서 자주 사용되는 기능입니다. 이 글에서는 **`classList`**와 **`setAttribute`/`getAttribute`**를 비교하고, 각각의 특징과 적합한 사용 사례를 설명합니다.

## classList: 현대적인 클래스 조작 방법

`classList`는 클래스 조작을 위해 설계된 API로, **특정 클래스 추가/제거/토글**에 최적화되어 있습니다.

### 주요 특징
- **간결하고 직관적**: 메서드를 통해 특정 클래스만 조작.
- **안전한 동작**: 기존 클래스에 영향을 주지 않음.
- **성능 최적화**: 브라우저에서 빠르게 동작.

### 주요 메서드
- `add(className)`: 클래스 추가.
- `remove(className)`: 클래스 제거.
- `toggle(className)`: 클래스 추가/제거 토글.
- `contains(className)`: 클래스 포함 여부 확인.

### 예제
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
`classList`는 코드가 명확하고, 유지보수에 용이한 것이 장점입니다.

---

## setAttribute / getAttribute: 범용 속성 조작

`setAttribute`와 `getAttribute`는 HTML 요소의 속성을 읽거나 설정하는 **범용적인 메서드**입니다. 클래스뿐만 아니라 ID, 데이터 속성 등 다양한 속성 조작이 가능합니다.

### 주요 특징
- **유연성**: 모든 속성을 처리 가능.
- **클래스 전체 조작**: 한 번에 전체 클래스를 변경할 때 유용.

### 클래스 조작 시의 단점
- **덮어쓰기 문제**: 기존 클래스가 모두 덮어써질 위험이 있음.
- **번거로운 작업**: 특정 클래스를 추가/제거하려면 문자열 처리가 필요.

### 예제
클래스 전체 변경:
``` js
const element = document.getElementById('box');

// 클래스 설정 (기존 클래스 덮어씀)
element.setAttribute('class', 'newClass');

// 클래스 읽기
const currentClass = element.getAttribute('class');
console.log(currentClass); // 'newClass'
```
기존 클래스 유지하면서 추가:
``` js
let classes = element.getAttribute('class'); // 기존 클래스 읽기
classes += ' newClass'; // 새로운 클래스 추가
element.setAttribute('class', classes); // 업데이트
```

---

## 언제 무엇을 사용해야 할까?

| 사용 사례                             | 추천 방법    |
|-------------------------------------|-------------|
| 특정 클래스만 추가/제거/토글해야 할 때   | `classList` |
| 클래스 외의 다른 속성을 조작해야 할 때   | `setAttribute` |
| 클래스 전체를 한 번에 설정해야 할 때    | `setAttribute` |
| 특정 클래스 포함 여부를 확인해야 할 때   | `classList` |

---

## 성능 비교

`classList`는 클래스 조작에 최적화되어 있어 더 빠르고 효율적입니다. 반면, `setAttribute`는 문자열 처리를 요구하므로 클래스 조작에서는 다소 비효율적일 수 있습니다.

---

## 결론

- **클래스 조작**에는 `classList`를 사용하는 것이 현대적이고 권장되는 방법입니다.
- **속성 조작**이 필요하거나 클래스 전체를 설정해야 한다면 `setAttribute`/`getAttribute`를 사용하는 것이 적합합니다.

**JavaScript로 HTML 요소 스타일을 조작할 때는 상황에 맞는 도구를 사용**하여 코드의 효율성과 유지보수를 높이는 것이 중요합니다.
