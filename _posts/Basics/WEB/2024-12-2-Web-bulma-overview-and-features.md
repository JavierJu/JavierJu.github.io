---  
title: "Bulma CSS 프레임워크: 개요와 주요 기능"  
excerpt: "Flexbox 기반의 현대적인 CSS 프레임워크 Bulma의 특징과 주요 기능을 코드 예제와 함께 알아봅니다."  
categories:  
  - Web  
tags:  
  - [CSS, Frontend, Web Design, Bulma, Responsive Design]  
permalink: /Web/bulma-overview-and-features/  
toc: true  
toc_sticky: true  
date: 2024-12-2  
last_modified_at: 2024-12-2  
---  

## Bulma 개요  

Bulma는 Flexbox 기반의 현대적인 CSS 프레임워크로, 반응형 디자인과 다양한 UI 컴포넌트를 간편하게 구현할 수 있도록 도와줍니다. 이 프레임워크는 클래스 기반으로 작성되어, HTML 마크업만으로도 다양한 스타일을 손쉽게 적용할 수 있습니다.  

## Bulma의 주요 특징  

- **Flexbox 기반**: 유연하고 강력한 레이아웃 구성이 가능합니다.  
- **반응형 디자인**: 화면 크기에 따라 유동적으로 레이아웃이 조정됩니다.  
- **컴포넌트 중심**: 버튼, 카드, 모달 등 다양한 UI 컴포넌트를 제공합니다.  
- **클래스 기반 접근**: CSS 작성 없이 HTML 클래스만으로 스타일을 설정할 수 있습니다.  

## 주요 기능과 코드 예제  

### 1. 컨테이너와 그리드 시스템  

``` html  
<div class="container">  
  <div class="columns">  
    <div class="column is-half">  
      <!-- 첫 번째 컬럼 내용 -->  
    </div>  
    <div class="column is-half">  
      <!-- 두 번째 컬럼 내용 -->  
    </div>  
  </div>  
</div>  
```  

- `container`: 페이지 중앙 정렬 및 패딩 제공.  
- `columns`: 여러 개의 열을 포함하는 그리드 컨테이너.  
- `column`: 개별 열을 나타내며, `is-half`로 크기 조절 가능.  

### 2. 버튼  

``` html  
<button class="button is-primary">Primary Button</button>  
<button class="button is-link">Link Button</button>  
<button class="button is-danger is-large">Large Danger Button</button>  
```  

- `is-primary`, `is-link`, `is-danger`: 색상 지정 클래스.  
- `is-large`, `is-small`: 버튼 크기 조정 클래스.  

### 3. 알림 메시지 (Notification)  

``` html  
<div class="notification is-info">  
  This is an info notification.  
</div>  
<div class="notification is-success">  
  This is a success notification.  
</div>  
```  

- `is-info`, `is-success`: 색상 지정.  

### 4. 카드 (Card)  

``` html  
<div class="card">  
  <div class="card-content">  
    <p class="title">Card Title</p>  
    <p class="content">This is the content of the card.</p>  
  </div>  
</div>  
```  

- `card`: 카드 컨테이너.  
- `card-content`: 카드의 내용 영역.  

### 5. 모달 (Modal)  

``` html  
<!-- 모달 버튼 -->  
<button class="button is-primary" id="modal-button">Show Modal</button>  

<!-- 모달 -->  
<div class="modal" id="modal">  
  <div class="modal-background"></div>  
  <div class="modal-content">  
    <div class="box">  
      <p>Modal Content</p>  
    </div>  
  </div>  
  <button class="modal-close is-large" id="modal-close" aria-label="close"></button>  
</div>  

<script>  
  // 모달 열기/닫기  
  document.getElementById('modal-button').addEventListener('click', function() {  
    document.getElementById('modal').classList.add('is-active');  
  });  
  document.getElementById('modal-close').addEventListener('click', function() {  
    document.getElementById('modal').classList.remove('is-active');  
  });  
</script>  
```  

- `modal`: 모달의 기본 구조.  
- `is-active`: 모달을 활성화하는 클래스.  

### 6. 반응형 디자인  

``` html  
<div class="columns is-mobile">  
  <div class="column">  
    <!-- 모바일 화면에서는 하나의 열 -->  
  </div>  
  <div class="column is-three-quarters">  
    <!-- 모바일 화면에서는 3/4 열 -->  
  </div>  
</div>  
```  

- `is-mobile`: 모바일 전용 레이아웃.  
- `is-three-quarters`: 3/4 너비 설정.  

## 결론  

Bulma는 직관적이고 간편한 CSS 프레임워크로, 현대적인 웹 디자인 요구 사항을 충족시킬 수 있는 다양한 도구와 기능을 제공합니다. Flexbox 기반의 강력한 레이아웃 시스템과 손쉽게 사용할 수 있는 UI 컴포넌트 덕분에 효율적인 웹 개발이 가능합니다.  
