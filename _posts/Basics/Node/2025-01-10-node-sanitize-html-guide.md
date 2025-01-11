---
title: "sanitize-html: Node.js에서 안전한 HTML 처리하기"
excerpt: "sanitize-html 라이브러리를 활용하여 HTML 문자열에서 XSS 공격을 방지하고 안전하게 사용자 입력을 처리하는 방법을 알아봅니다."
categories:
  - Node
  - Security
tags:
  - [JavaScript, Node.js, XSS, Security, HTML]
permalink: /node/sanitize-html-guide/
toc: true
toc_sticky: true
date: 2025-01-10
last_modified_at: 2025-01-10
---

`sanitize-html`는 Node.js 환경에서 HTML 문자열을 안전하게 처리하는 라이브러리입니다. 사용자 입력에서 잠재적으로 위험한 콘텐츠(예: `<script>` 태그)를 제거하고, 허용된 태그와 속성만 남겨 XSS(Cross-Site Scripting) 공격을 방지합니다. 이 글에서는 `sanitize-html`의 주요 기능, 설치 방법, 사용 예제, 그리고 활용 사례를 다룹니다.

## 주요 기능
- **HTML 필터링**: HTML 문자열에서 허용되지 않은 태그와 속성을 제거.
- **태그와 속성 화이트리스트**: 허용할 태그와 속성을 사용자 지정 가능.
- **스타일 속성 제어**: 특정 스타일 속성만 허용하거나 제거.
- **XSS 방지**: `<script>` 태그, `on*` 이벤트 핸들러 속성 등을 제거하여 XSS 공격 차단.

## 설치
`sanitize-html`는 npm을 통해 간단히 설치할 수 있습니다.

```bash
npm install sanitize-html
```

## 기본 사용법
다음은 `sanitize-html`의 기본 사용 예제입니다:

```javascript
const sanitizeHtml = require('sanitize-html');

const dirtyHtml = `
  <div>
    <script>alert('XSS!');</script>
    <h1 style="color: red;">Hello World</h1>
    <a href="javascript:alert('XSS!')">Click me</a>
  </div>
`;

const cleanHtml = sanitizeHtml(dirtyHtml);

console.log(cleanHtml);
// 결과: <div><h1>Hello World</h1></div>
```

`sanitize-html`은 기본적으로 안전하지 않은 태그나 속성을 제거합니다. 위 예제에서는 `<script>` 태그와 `javascript:` 스키마를 제거합니다.

## 커스텀 설정
허용할 태그와 속성을 지정하려면 옵션을 추가할 수 있습니다.

```javascript
const cleanHtml = sanitizeHtml(dirtyHtml, {
  allowedTags: ['h1', 'h2', 'p', 'a'], // 허용할 태그
  allowedAttributes: {
    a: ['href'], // 특정 태그에 허용할 속성
  },
  allowedSchemes: ['http', 'https'], // 링크의 허용된 스키마
});

console.log(cleanHtml);
// 결과: <h1>Hello World</h1><a href="https://example.com">Click me</a>
```

## 고급 옵션
### 허용할 스타일 속성
다음과 같이 특정 스타일 속성만 허용할 수 있습니다:

```javascript
const cleanHtml = sanitizeHtml(dirtyHtml, {
  allowedTags: ['h1'],
  allowedAttributes: {
    h1: ['style'],
  },
  allowedStyles: {
    '*': {
      color: [/^red$/], // 'red'만 허용
      'font-size': [/^\d+px$/], // 픽셀 단위 폰트 크기만 허용
    },
  },
});
```

### 태그 변환
`transformTags` 옵션을 사용하면 태그를 변환하거나 수정할 수 있습니다.

```javascript
const cleanHtml = sanitizeHtml(dirtyHtml, {
  transformTags: {
    'h1': (tagName, attribs) => {
      return {
        tagName: 'h2',
        attribs: {
          class: 'heading',
        },
      };
    },
  },
});

console.log(cleanHtml);
// 결과: <h2 class="heading">Hello World</h2>
```

## 활용 사례
`sanitize-html`은 다양한 시나리오에서 유용하게 사용됩니다:
- **댓글 시스템**: 사용자가 입력한 HTML을 안전하게 렌더링.
- **WYSIWYG 편집기**: 사용자 입력 내용을 정리하여 저장.
- **데이터 정화**: HTML 형식 데이터를 저장하기 전에 정리.

## 참고 자료
- 공식 문서: [sanitize-html GitHub](https://github.com/apostrophecms/sanitize-html)
- `sanitize-html`의 최신 기능과 변경 사항은 공식 문서를 참고하세요.

