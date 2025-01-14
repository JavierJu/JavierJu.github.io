---
title: "EJS로 Bootstrap 이미지 슬라이더에서 첫 번째 이미지만 Active 설정하기"
excerpt: "Bootstrap 이미지 슬라이더를 구현할 때 첫 번째 이미지만 `active` 클래스를 적용하고 나머지 이미지는 제외하는 방법에 대해 알아봅니다."
categories:
  - Frontend
  - EJS
  - Bootstrap
  - Web

tags:
  - [Bootstrap, EJS, 이미지 슬라이더, Frontend]
permalink: /web/bootstrap-carousel-active/
toc: true
toc_sticky: true
date: 2025-01-08
last_modified_at: 2025-01-08
---

이미지 슬라이더(Carousel)를 구현할 때, Bootstrap은 `active` 클래스를 통해 슬라이더의 첫 번째 이미지를 기본 활성 상태로 만듭니다. EJS와 같은 템플릿 엔진을 사용할 경우, 첫 번째 이미지만 `active` 클래스를 적용하고 나머지 이미지는 제외하는 조건을 설정해야 합니다.

이 글에서는 이를 구현하는 세 가지 방법을 소개하고, 가장 적합한 방법을 추천합니다.

---

## 방법 1: `forEach`와 인덱스 사용 (추천)

```ejs
<% campground.images.forEach((img, i) => { %>
    <div class="carousel-item <%= i === 0 ? 'active' : '' %>">
        <img src="<%= img.url %>" class="d-block w-100" alt="">
    </div>
<% }) %>
```

### 설명:
- `forEach` 메서드의 두 번째 매개변수인 `i`를 활용해 인덱스를 확인합니다.
- 첫 번째 항목(`i === 0`)에는 `active` 클래스를 추가하고, 나머지 항목은 공백으로 둡니다.

---

## 방법 2: 전통적인 `for` 루프 사용

```ejs
<% for (let i = 0; i < campground.images.length; i++) { %>
    <div class="carousel-item <%= i === 0 ? 'active' : '' %>">
        <img src="<%= campground.images[i].url %>" class="d-block w-100" alt="">
    </div>
<% } %>
```

### 설명:
- 전통적인 `for` 루프를 사용하여 배열을 순회합니다.
- 첫 번째 항목(`i === 0`)에만 `active` 클래스를 추가합니다.
- 명시적으로 인덱스를 관리해야 합니다.

---

## 방법 3: `forEach`와 수동 인덱스 관리

```ejs
<% let index = 0; %>
<% campground.images.forEach(img => { %>
    <div class="carousel-item <%= index === 0 ? 'active' : '' %>">
        <img src="<%= img.url %>" class="d-block w-100" alt="">
    </div>
    <% index++; %>
<% }); %>
```

### 설명:
- 루프 외부에서 `index` 변수를 선언하여 수동으로 관리합니다.
- 첫 번째 항목(`index === 0`)에만 `active` 클래스를 추가합니다.
- 추가 변수를 선언하고 관리해야 하므로 다소 복잡합니다.

---

## 어떤 방법을 선택해야 할까?

### 추천: **방법 1**
- 가장 간결하고 가독성이 좋습니다.
- `forEach` 메서드가 인덱스를 기본적으로 제공하므로 추가 변수를 선언할 필요가 없습니다.

### 예외 상황:
- **배열이 아닌 데이터 구조를 순회하거나 인덱스를 직접 조작해야 하는 경우**에는 방법 2나 3을 고려할 수 있습니다.
- 방법 2는 전통적인 루프 방식으로 유연성이 높으며, 방법 3은 `forEach`를 사용하면서도 수동으로 인덱스를 관리할 수 있는 장점이 있습니다.

---

Bootstrap 이미지 슬라이더와 같은 인터페이스를 만들 때는 코드의 간결성과 가독성을 고려하는 것이 중요합니다. 위 세 가지 방법 중 적합한 방법을 선택하여 프로젝트에 적용해 보세요.

