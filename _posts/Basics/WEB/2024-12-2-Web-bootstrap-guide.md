---
title: "CSS 프레임워크 Bootstrap: 기능과 코드 예제"
excerpt: "Bootstrap의 주요 기능과 자주 사용하는 코드 예제를 통해 웹 개발을 더 효율적으로 만들어 보세요."
categories:
  - Web
tags:
  - [CSS, Bootstrap, Web Development, Responsive Design, UI Components]
permalink: /Web/bootstrap-guide/
toc: true
toc_sticky: true
date: 2024-12-2
last_modified_at: 2024-12-2
---

## Bootstrap이란?

Bootstrap은 웹 개발을 빠르고 쉽게 할 수 있도록 도와주는 인기 있는 CSS 프레임워크입니다. 모바일 우선(mobile-first) 접근 방식을 채택하며, HTML, CSS, JavaScript 구성 요소로 이루어져 있습니다. 이를 사용하면 반응형 웹 디자인, 다양한 UI 컴포넌트, 그리고 다양한 레이아웃 옵션을 손쉽게 구현할 수 있습니다.

---

## Bootstrap 주요 기능

1. **반응형 디자인 (Responsive Design)**  
   다양한 화면 크기에 맞춰 디자인을 자동으로 조정합니다.

2. **그리드 시스템 (Grid System)**  
   12열 기반의 유연한 레이아웃 구성이 가능합니다.

3. **컴포넌트 (Components)**  
   버튼, 네비게이션 바, 카드 등 다양한 UI 요소를 제공합니다.

4. **유틸리티 클래스 (Utility Classes)**  
   여백, 폰트 크기, 정렬 등을 쉽게 적용할 수 있는 클래스를 제공합니다.

---

## Bootstrap 사용 예제

### 1. 기본 설정
Bootstrap을 사용하려면 아래 코드를 HTML 파일에 추가하세요.

``` html
<!-- Bootstrap CSS -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">

<!-- Bootstrap JavaScript -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"></script>
```

---

### 2. 그리드 시스템 사용
그리드 시스템은 `container`, `row`, `col` 클래스를 사용하여 레이아웃을 만듭니다.

``` html
<div class="container">
  <div class="row">
    <div class="col-4">
      <div class="bg-primary text-white p-3">컬럼 1</div>
    </div>
    <div class="col-4">
      <div class="bg-secondary text-white p-3">컬럼 2</div>
    </div>
    <div class="col-4">
      <div class="bg-success text-white p-3">컬럼 3</div>
    </div>
  </div>
</div>
```

---

### 3. 버튼
다양한 색상과 크기의 버튼을 쉽게 추가할 수 있습니다.

``` html
<button class="btn btn-primary">기본 버튼</button>
<button class="btn btn-secondary">보조 버튼</button>
<button class="btn btn-success">성공 버튼</button>
<button class="btn btn-danger">위험 버튼</button>
```

---

### 4. 네비게이션 바
네비게이션 바는 메뉴를 구성하는 데 유용합니다.

``` html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="#">로고</a>
  <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
    <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNav">
    <ul class="navbar-nav">
      <li class="nav-item active">
        <a class="nav-link" href="#">홈</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">링크</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">서비스</a>
      </li>
    </ul>
  </div>
</nav>
```

---

### 5. 카드
이미지와 텍스트를 포함한 카드를 손쉽게 만들 수 있습니다.

``` html
<div class="card" style="width: 18rem;">
  <img src="https://via.placeholder.com/150" class="card-img-top" alt="이미지">
  <div class="card-body">
    <h5 class="card-title">카드 제목</h5>
    <p class="card-text">이곳에 카드 내용이 들어갑니다.</p>
    <a href="#" class="btn btn-primary">더 보기</a>
  </div>
</div>
```

---

### 6. 모달
팝업 형태의 대화상자를 추가할 수 있습니다.

``` html
<button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
  모달 열기
</button>

<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">모달 제목</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
      </div>
      <div class="modal-body">
        모달 본문 내용
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">닫기</button>
        <button type="button" class="btn btn-primary">저장</button>
      </div>
    </div>
  </div>
</div>
```

---

### 7. 툴팁
사용자에게 추가 정보를 제공할 수 있습니다.

``` html
<button type="button" class="btn btn-secondary" data-bs-toggle="tooltip" data-bs-placement="top" title="툴팁 텍스트">
  툴팁 버튼
</button>

<script>
  var tooltipTriggerList = [].slice.call(document.querySelectorAll('[data-bs-toggle="tooltip"]'));
  tooltipTriggerList.map(function (tooltipTriggerEl) {
    return new bootstrap.Tooltip(tooltipTriggerEl);
  });
</script>
```

---

## 결론

Bootstrap은 강력하고 효율적인 CSS 프레임워크로, 다양한 UI 컴포넌트와 반응형 디자인을 제공합니다. 이를 통해 개발 속도를 높이고, 사용자 친화적인 웹 페이지를 손쉽게 만들 수 있습니다.
