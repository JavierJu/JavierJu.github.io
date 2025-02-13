---
title: "npm-run-all: NPM 스크립트 효율적으로 관리하기"
excerpt: "npm-run-all을 사용하여 여러 NPM 스크립트를 효율적으로 관리하는 방법을 알아봅니다. 병렬 실행, 순차 실행, 패턴 매칭 등을 코드 예제와 함께 설명합니다."
categories:
  - Node
  - Development Tools
tags:
  - [JavaScript, Node.js, npm, 개발 도구, 스크립트 관리]
permalink: /node/npm-run-all/
toc: true
toc_sticky: true
date: 2025-01-25
last_modified_at: 2025-01-25
---

## npm-run-all: NPM 스크립트 효율적으로 관리하기

`npm-run-all`은 여러 개의 npm 스크립트를 **순차적**으로 또는 **병렬**로 실행할 수 있도록 도와주는 강력한 도구입니다. 이 패키지는 복잡한 빌드 프로세스나 다양한 작업을 쉽게 관리할 수 있게 해줍니다.

이 글에서는 `npm-run-all`의 설치 방법, 주요 기능, 그리고 사용법을 코드 예제와 함께 살펴보겠습니다.

---

## 1. npm-run-all 설치하기

다음 명령어를 실행하여 `npm-run-all`을 개발 의존성으로 설치합니다:

```bash
npm install npm-run-all --save-dev
```

---

## 2. 주요 기능

### 2.1 순차 실행
`npm-run-all`의 기본 동작은 여러 스크립트를 **순차적으로 실행**하는 것입니다.

예를 들어, 다음과 같은 npm 스크립트를 `package.json`에 정의할 수 있습니다:

```json
{
  "scripts": {
    "build": "npm-run-all clean lint test",
    "clean": "rimraf dist",
    "lint": "eslint src",
    "test": "jest"
  }
}
```

위 설정에서 `npm run build`를 실행하면 `clean`, `lint`, `test` 스크립트가 **순서대로** 실행됩니다.

```bash
npm run build
```

### 2.2 병렬 실행
`--parallel` 옵션을 사용하면 여러 스크립트를 **동시에 실행**할 수 있습니다.

예를 들어, 다음과 같은 설정을 추가할 수 있습니다:

```json
{
  "scripts": {
    "build": "npm-run-all --parallel clean lint test",
    "clean": "rimraf dist",
    "lint": "eslint src",
    "test": "jest"
  }
}
```

위 설정에서 `npm run build`를 실행하면 `clean`, `lint`, `test` 스크립트가 **동시에** 실행됩니다.

```bash
npm run build
```

> **참고**: 병렬 실행 시 로그가 뒤섞여 보일 수 있으므로, 작업 간 출력 구분이 필요하다면 추가 설정을 고려하세요.

### 2.3 특정 패턴 실행
`npm-run-all`은 **패턴 매칭**을 통해 특정 이름 패턴을 가진 스크립트를 실행할 수 있습니다.

다음과 같은 예를 보세요:

```json
{
  "scripts": {
    "build": "npm-run-all --run build:*",
    "build:js": "webpack",
    "build:css": "sass src/styles.scss dist/styles.css"
  }
}
```

위 설정에서 `npm run build`를 실행하면 `build:js`와 `build:css`가 실행됩니다.

```bash
npm run build
```

---

## 3. 실전 예제

다음은 실제 프로젝트에서 자주 사용되는 예제입니다:

```json
{
  "scripts": {
    "start": "npm-run-all --parallel server client",
    "server": "nodemon server.js",
    "client": "react-scripts start",
    "build": "npm-run-all clean lint build:*",
    "build:js": "webpack",
    "build:css": "sass src/styles.scss dist/styles.css",
    "clean": "rimraf dist",
    "lint": "eslint src"
  }
}
```

- `npm run start`: 백엔드(`server.js`)와 프론트엔드(`react-scripts`)를 동시에 실행합니다.
- `npm run build`: `clean`, `lint`, 그리고 `build:js`, `build:css`를 순차적으로 실행합니다.

---

## 4. 정리

`npm-run-all`은 다음과 같은 상황에서 특히 유용합니다:

- 여러 작업을 순차적으로 실행해야 할 때
- 병렬로 실행되는 작업을 관리해야 할 때
- 패턴을 기반으로 작업을 실행해야 할 때

이 도구를 사용하면 `package.json`의 스크립트를 더 깔끔하고 체계적으로 관리할 수 있습니다. 지금 바로 프로젝트에 적용해 보세요!

