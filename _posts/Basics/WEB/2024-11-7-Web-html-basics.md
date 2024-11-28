---
title: "HTML의 기초 요소 정리: 단락, 제목, 목록, 앵커 태그, 이미지, 주석"
excerpt: "HTML 기초 요소를 이해하고 웹 페이지의 기본 구조를 작성하는 방법을 알아봅니다."
categories:
  - Web
tags:
  - [HTML, Web Development, Beginners]
permalink: /Web/html-basics/
toc: true
toc_sticky: true
date: 2024-11-7
last_modified_at: 2024-11-7
---

HTML(하이퍼텍스트 마크업 언어)은 웹 페이지를 구성하고 내용을 구조화하는 데 사용되는 가장 기본적인 언어입니다. 이번 포스트에서는 HTML의 기초 요소들을 정리하고, 이를 활용해 간단한 웹 페이지를 작성하는 방법을 소개합니다.

## **HTML의 기본 구조**
HTML 문서는 다음과 같은 기본 구조를 가집니다:
``` html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>문서 제목</title>
</head>
<body>
    <!-- 페이지 내용은 여기 작성 -->
</body>
</html>
```

- `<!DOCTYPE html>`: HTML5 문서임을 선언합니다.
- `<html>`: 문서의 루트(root) 요소로, 모든 HTML 코드가 포함됩니다.
- `<head>`: 문서의 메타데이터(제목, 인코딩, 외부 리소스)를 정의합니다.
- `<body>`: 웹 페이지에 표시되는 모든 내용을 포함합니다.

---

## **단락 요소**
단락 요소는 문서를 논리적인 단락으로 나누는 데 사용됩니다.

- **`<p>`**: 단락을 나타내는 태그입니다.
``` html
<p>이것은 단락입니다.</p>
```

---

## **제목 요소**
제목 요소는 콘텐츠의 중요도와 계층 구조를 정의합니다.

- `<h1>`에서 `<h6>`까지 6단계의 제목 태그가 있습니다. `<h1>`이 가장 중요한 제목, `<h6>`이 가장 덜 중요한 제목입니다.
``` html
<h1>가장 중요한 제목</h1>
<h2>두 번째로 중요한 제목</h2>
<h3>세 번째 제목</h3>
```

---

## **HTML 상용구**
HTML 문서에서 자주 사용하는 기본적인 태그들입니다.

### **수평선**
- `<hr>`: 단락 사이를 구분하는 수평선을 표시합니다.
``` html
<p>첫 번째 단락</p>
<hr>
<p>두 번째 단락</p>
```

### **강조**
- `<strong>`: 강한 강조를 표시합니다.
- `<em>`: 약한 강조를 표시합니다.
``` html
<p>이 문장은 <strong>강하게</strong> 강조됩니다.</p>
<p>이 문장은 <em>부드럽게</em> 강조됩니다.</p>
```

---

## **목록 요소**
HTML에서 목록은 순서가 있는 목록과 순서가 없는 목록으로 나뉩니다.

### 순서 없는 목록 (`<ul>`)
``` html
<ul>
    <li>항목 1</li>
    <li>항목 2</li>
</ul>
```

### 순서 있는 목록 (`<ol>`)
``` html
<ol>
    <li>첫 번째 항목</li>
    <li>두 번째 항목</li>
</ol>
```

---

## **앵커 태그**
하이퍼링크를 생성하는 태그로, `href` 속성으로 이동할 URL을 지정합니다.

### 기본 사용법
``` html
<a href="https://www.example.com">예제 사이트로 이동</a>
```

### 주요 속성
- `href`: 링크 대상 URL.
- `target="_blank"`: 링크를 새 탭에서 열도록 지정.

---

## **이미지 삽입**
이미지는 `<img>` 태그를 사용하며, `src` 속성에 이미지 경로를 지정합니다.

### 기본 사용법
``` html
<img src="example.jpg" alt="설명 텍스트" width="300" height="200">
```

### 주요 속성
- `src`: 이미지 파일의 경로.
- `alt`: 대체 텍스트(이미지가 로드되지 않을 때 표시됨).
- `width`, `height`: 이미지 크기를 지정.

---

## **주석**
HTML 주석은 코드에 설명을 추가하거나 일시적으로 코드를 비활성화하는 데 사용됩니다.

### 사용법
``` html
<!-- 이것은 주석입니다 -->
<p>이것은 HTML 요소입니다.</p>
```

---

## **예제 전체 코드**
위 내용을 종합하여 HTML 기초 요소를 활용한 간단한 예제는 다음과 같습니다:
``` html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML 기초 예제</title>
</head>
<body>
    <h1>HTML 기초</h1>
    <p>HTML은 웹 문서를 작성하는 데 사용됩니다.</p>
    <hr>

    <h2>목록</h2>
    <ul>
        <li>HTML</li>
        <li>CSS</li>
        <li>JavaScript</li>
    </ul>

    <h2>이미지</h2>
    <img src="example.jpg" alt="예제 이미지" width="300">

    <h2>링크</h2>
    <a href="https://www.example.com" target="_blank">예제 사이트로 이동</a>

    <!-- 이것은 주석입니다 -->
</body>
</html>
```
