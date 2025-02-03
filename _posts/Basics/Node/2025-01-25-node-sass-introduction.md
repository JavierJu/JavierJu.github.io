---
title: "Node-sass: Sass 컴파일러 소개 및 대체 라이브러리 추천"
excerpt: "node-sass를 이용한 Sass 컴파일 방법과 대체 라이브러리인 Dart Sass를 사용하는 방법을 코드 예제와 함께 설명합니다."
categories:
  - Node
  - Frontend
  - Sass
  - Tools
tags:
  - [Node.js, Sass, CSS, node-sass, Frontend, Tools]
permalink: /node/node-sass-introduction/
toc: true
toc_sticky: true
date: 2025-01-25
last_modified_at: 2025-01-25
---

`node-sass`는 Node.js 환경에서 Sass(Syntactically Awesome Stylesheets)를 컴파일하기 위한 라이브러리입니다. Sass는 CSS의 확장 언어로, 변수를 사용하거나 중첩 규칙, 믹스인 등을 통해 더 간결하고 효율적인 스타일 시트를 작성할 수 있습니다.

## 주요 특징

- **Sass와 SCSS 컴파일**: `.sass` 또는 `.scss` 파일을 `.css`로 컴파일합니다.
- **속도**: `node-sass`는 C++로 구현되어 있어 빠른 컴파일 속도를 제공합니다.
- **호환성**: 다양한 Node.js 버전과 호환됩니다.

## 설치 방법

아래 명령어를 사용하여 `node-sass`를 설치할 수 있습니다.

```bash
npm install node-sass
```

## 기본 사용법

`node-sass`를 사용하여 Sass 파일을 컴파일하는 간단한 예제입니다.

```javascript
const sass = require('node-sass');

sass.render({
  file: 'path/to/your/file.scss', // 컴파일할 Sass 파일 경로
}, (err, result) => {
  if (err) {
    console.error(err);
  } else {
    console.log(result.css.toString()); // 컴파일된 CSS 출력
  }
});
```

## 대체 라이브러리: Dart Sass (`sass`)

`node-sass`는 현재 유지보수가 중단된 상태로, 더 이상 추천되지 않습니다. 대신 [Dart Sass](https://sass-lang.com/dart-sass)를 사용하는 것이 권장됩니다. Dart Sass는 Sass의 공식 구현체로, 최신 기능과 향후 업데이트를 제공합니다.

### 설치

아래 명령어를 사용하여 Dart Sass(`sass`)를 설치할 수 있습니다.

```bash
npm install sass
```

### Dart Sass 사용법

Dart Sass를 사용하여 Sass 파일을 컴파일하는 예제입니다.

```javascript
const sass = require('sass');

sass.render({
  file: 'path/to/your/file.scss', // 컴파일할 Sass 파일 경로
}, (err, result) => {
  if (err) {
    console.error(err);
  } else {
    console.log(result.css.toString()); // 컴파일된 CSS 출력
  }
});
```

## 주요 차이점

| **특징**              | **node-sass**                       | **Dart Sass**                   |
|-----------------------|-------------------------------------|---------------------------------|
| 구현 언어             | C++                                 | Dart                            |
| 설치 및 빌드 문제     | 종종 발생                           | 거의 없음                       |
| 유지보수 상태         | 중단                                | 적극적으로 유지보수 중          |
| 최신 Sass 기능 지원   | 제한적                              | 완전 지원                        |

## 결론

`node-sass`는 과거에는 유용한 도구였으나, 유지보수가 중단되면서 Dart Sass(`sass`)로 전환하는 것이 현재로서는 더 좋은 선택입니다. 특히, Dart Sass는 더 많은 기능과 더 나은 안정성을 제공하므로, 새로운 프로젝트나 기존 프로젝트의 업그레이드에 Dart Sass를 사용하는 것을 적극 추천합니다.

