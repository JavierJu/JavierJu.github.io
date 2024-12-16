---
title: "HTML 시맨틱(Semantics)과 HTML5: 개념과 활용법"
excerpt: "HTML 시맨틱의 중요성과 HTML5의 주요 시맨틱 요소, 블록/인라인 요소, 엔티티 코드, 시맨틱 마크업의 사용법까지 자세히 알아봅니다."
categories:
  - Web
tags:
  - [HTML, Web Development, Semantics, Accessibility, SEO]
permalink: /Web/html5-semantics-guide/
toc: true
toc_sticky: true
date: 2024-11-7
last_modified_at: 2024-11-7
---

## HTML의 시맨틱(Semantics)이란?

HTML 시맨틱은 코드에서 태그를 통해 문서의 구조와 의미를 명확히 하는 개념입니다. 시맨틱 요소를 사용하면 브라우저, 검색엔진, 그리고 개발자가 콘텐츠의 목적과 구조를 쉽게 이해할 수 있습니다.

---

## HTML5란?

HTML5는 HTML 표준의 다섯 번째 버전으로, 웹의 발전과 요구 사항에 맞춰 개선된 웹 마크업 언어입니다.

### 주요 특징:
- **새로운 시맨틱 요소 추가**: `<header>`, `<footer>`, `<article>`, `<section>` 등.
- **멀티미디어 지원**: `<audio>`와 `<video>`를 통해 플러그인 없이 멀티미디어 재생 가능.
- **새로운 API**: `<canvas>`를 활용한 그래픽 처리, 오프라인 저장소, 지리 위치 정보 등 제공.
- **단순화**: `<font>`, `<center>` 같은 불필요한 태그와 속성 제거.

---

## HTML의 요소 종류

### 블록 요소 (Block Element)
- 한 줄 전체를 차지하며, 독립적인 영역을 만듭니다.
- 예: `<div>`, `<p>`, `<h1>`, `<section>`, `<article>`
- 스타일 적용 전에는 디폴트로 **줄바꿈**이 이루어짐.

예:

``` html
<div>
    <h1>제목</h1>
    <p>내용입니다.</p>
</div>
```

### 인라인 요소 (Inline Element)
- 콘텐츠 내부에 삽입되며, 다른 요소와 같은 줄에 위치합니다.
- 예: `<span>`, `<a>`, `<strong>`, `<img>`
- 줄바꿈 없이 콘텐츠를 꾸미거나 하이퍼링크 등 특정 기능 추가에 사용.

예:

``` html
<p>이 문장에는 <strong>강조</strong>된 단어가 포함되어 있습니다.</p>
```

### 기타 요소
- **HTML5 시맨틱 요소**: `<header>`, `<footer>`, `<nav>`, `<article>`, `<section>`.
- **폼 요소**: `<input>`, `<button>`, `<form>`, `<label>`.
- **미디어 요소**: `<audio>`, `<video>`, `<picture>`.

---

## HTML 엔티티 코드

특수 문자나 기호를 표시하기 위해 HTML 엔티티를 사용합니다.

| 문자 | 엔티티 코드  | 결과  |
|------|--------------|-------|
| `<`  | `&lt;`       | <     |
| `>`  | `&gt;`       | >     |
| `&`  | `&amp;`      | &     |
| `"`  | `&quot;`     | "     |
| `©`  | `&copy;`     | ©     |

---

## 시맨틱 마크업이란?

시맨틱 마크업은 태그 이름만 보고도 그 역할이나 의미를 이해할 수 있도록 작성된 HTML입니다.

### 장점:
1. **접근성 개선**: 스크린 리더 등이 콘텐츠를 더 잘 이해.
2. **SEO 향상**: 검색엔진이 콘텐츠 구조를 명확히 분석.
3. **가독성 향상**: 개발자 간 협업이나 유지보수에 유리.

### 비시맨틱 태그와 비교:
- **비시맨틱 태그**: `<div>`, `<span>` - 구조를 설명하지 않음.
- **시맨틱 태그**: `<article>`, `<footer>` - 역할을 설명.

---

## HTML 시맨틱 요소 사용법

### 문서 구조 잡기

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML5 시맨틱 구조</title>
</head>
<body>
    <header>
        <h1>웹사이트 제목</h1>
        <nav>
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <section>
            <h2>섹션 제목</h2>
            <p>이곳은 섹션 콘텐츠입니다.</p>
        </section>
        <article>
            <h2>아티클 제목</h2>
            <p>아티클 내용입니다.</p>
        </article>
    </main>
    <footer>
        <p>&copy; 2024 웹사이트</p>
    </footer>
</body>
</html>
```

### 폼 예시

``` html
<form action="/submit" method="POST">
    <label for="name">이름:</label>
    <input type="text" id="name" name="name">
    <button type="submit">전송</button>
</form>
```

---

## 결론

HTML 시맨틱은 웹 페이지의 구조를 명확히 하고 검색엔진과 사용자 경험을 향상시키는 데 중요한 역할을 합니다. HTML5에서는 시맨틱 요소가 강화되었으니 이를 적극 활용해 의미 있는 문서를 작성하는 것이 좋습니다.
