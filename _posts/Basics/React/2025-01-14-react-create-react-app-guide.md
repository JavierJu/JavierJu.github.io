---
title: "Create React App: 완벽 가이드"
excerpt: "Create React App(CRA)을 사용해 React 애플리케이션을 빠르게 생성하고 시작하는 방법을 알아봅니다. 설치부터 기본 구조, 주요 명령어까지 자세히 설명합니다."
categories:
  - React
  - Frontend
tags:
  - [React, Create React App, JavaScript, Frontend, 개발 도구]
permalink: /react/create-react-app-guide/
toc: true
toc_sticky: true
date: 2025-01-14
last_modified_at: 2025-01-14
---

**Create React App (CRA)**는 React 애플리케이션을 쉽게 생성하고 시작할 수 있도록 돕는 공식 도구입니다. 이 도구를 사용하면 React 개발에 필요한 초기 설정(예: Webpack, Babel 등)을 신경 쓸 필요 없이 빠르게 프로젝트를 시작할 수 있습니다. 아래는 CRA에 대한 자세한 설명입니다.

---

## 주요 특징

1. **빠른 시작**  
   React 애플리케이션의 기본 구조와 설정을 자동으로 생성합니다. 따라서 설정에 시간을 낭비하지 않고 바로 개발에 집중할 수 있습니다.

2. **사전 구성된 도구**  
   CRA는 Babel, Webpack, ESLint와 같은 중요한 개발 도구를 기본적으로 설정하여 제공하며, 이를 사용자가 따로 설정할 필요가 없습니다.

3. **Zero Configuration**  
   프로젝트를 시작하는 데 별도의 설정이 필요하지 않으며, "out-of-the-box" 환경을 제공합니다.

4. **React 최신 기능 지원**  
   CRA는 React의 최신 버전 및 기능과 잘 통합되어 항상 최신 React 환경에서 작업할 수 있습니다.

5. **eject 기능 제공**  
   필요할 경우 기본 설정을 커스터마이징할 수 있도록 `eject` 명령어를 제공합니다.

---

## 설치 및 시작

### 1. Create React App 설치

```bash
npx create-react-app my-app
```

- `npx`: Node.js에서 제공하는 명령어로, 최신 버전의 CRA를 실행합니다.
- `my-app`: 생성할 프로젝트의 폴더 이름입니다.

또는:

```bash
npm init react-app my-app
```

**참고:** Node.js와 npm이 설치되어 있어야 합니다.

### 2. 프로젝트 디렉토리로 이동

```bash
cd my-app
```

### 3. 개발 서버 실행

```bash
npm start
```

- 기본적으로 [http://localhost:3000](http://localhost:3000)에서 개발 서버가 실행됩니다.

---

## 폴더 구조

CRA로 생성된 프로젝트의 기본 폴더 구조는 다음과 같습니다:

```plaintext
my-app/
├── node_modules/
├── public/
│   ├── index.html
│   └── favicon.ico
├── src/
│   ├── App.css
│   ├── App.js
│   ├── App.test.js
│   ├── index.css
│   ├── index.js
│   ├── logo.svg
│   └── reportWebVitals.js
├── .gitignore
├── package.json
├── README.md
└── yarn.lock (또는 package-lock.json)
```

### 주요 파일 및 폴더 설명

- **`public/index.html`**: 애플리케이션의 루트 HTML 파일입니다. React 컴포넌트는 이 파일 안의 `div` 태그(id: `root`)에 렌더링됩니다.
- **`src/index.js`**: 애플리케이션의 진입점(entry point) 파일입니다. ReactDOM을 사용하여 컴포넌트를 렌더링합니다.
- **`src/App.js`**: 기본 App 컴포넌트입니다.
- **`src/`**: React 컴포넌트와 스타일 파일 등을 저장하는 폴더입니다.

---

## 주요 명령어

CRA 프로젝트는 다음과 같은 npm 명령어를 제공합니다:

### 1. 개발 서버 실행

```bash
npm start
```

- 개발 중 애플리케이션을 로컬 서버에서 실행합니다.

### 2. 프로덕션 빌드

```bash
npm run build
```

- 최적화된 프로덕션 빌드를 생성합니다. 결과물은 `build` 폴더에 저장됩니다.

### 3. 테스트 실행

```bash
npm test
```

- Jest를 사용하여 테스트를 실행합니다.

### 4. eject (설정 커스터마이징)

```bash
npm run eject
```

- CRA 기본 설정(Webpack, Babel 등)을 추출하여 사용자 정의할 수 있습니다.
- **주의**: `eject`를 실행하면 되돌릴 수 없습니다.

---

## CRA 사용 시 장점과 단점

### 장점

- **빠른 초기 설정**: 설정 없이 React 프로젝트를 바로 시작할 수 있습니다.
- **최적화된 도구 제공**: Webpack, Babel 등의 최신 설정을 포함합니다.
- **커뮤니티 지원**: React 공식 도구로서 많은 자료와 지원을 받을 수 있습니다.
- **확장 가능성**: eject를 통해 설정을 커스터마이징할 수 있습니다.

### 단점

- **eject 이후 복잡성 증가**: eject를 실행하면 관리해야 할 설정 파일이 많아질 수 있습니다.
- **제한된 설정**: eject를 하지 않는 이상 Webpack 등의 설정을 직접 변경할 수 없습니다.
- **무거운 의존성**: 기본적으로 많은 의존성을 설치하므로 프로젝트 크기가 커질 수 있습니다.

---

CRA는 React 개발을 시작하는 데 가장 쉬운 방법 중 하나이며, 특히 초보자에게 적합합니다. 그러나 프로젝트가 커지고 요구사항이 복잡해질 경우 설정을 직접 관리해야 할 수도 있습니다.

