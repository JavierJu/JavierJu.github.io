---
title: "Vite 앱 사용법: 설치, 구조, 설정과 활용 방법 정리"
excerpt: "Vite 앱의 설치 방법, 디렉토리 구조, 주요 설정 및 활용 방법을 코드 예제와 함께 자세히 설명합니다."
categories:
  - React
  - Vite
  - Frontend
tags:
  - [Vite, JavaScript, Frontend, 웹 개발, 빌드 도구]
permalink: /react/frontend-vite-guide/
toc: true
toc_sticky: true
date: 2025-01-14
last_modified_at: 2025-01-14
---

## Vite란 무엇인가?

Vite는 **빠르고 효율적인 개발 환경**을 제공하기 위해 설계된 **프론트엔드 빌드 도구**입니다. 주로 React, Vue, Svelte, Vanilla JavaScript 등 다양한 프레임워크와 함께 사용됩니다. Vite는 ES 모듈 기반의 빠른 개발 서버와, Rollup 기반의 빌드 프로세스를 제공합니다.

---

## 주요 특징

1. **빠른 개발 서버**
   - 브라우저의 네이티브 ES 모듈 기능을 활용하여 즉각적인 HMR(Hot Module Replacement)을 제공합니다.
2. **빠른 빌드**
   - Rollup을 기반으로 최적화된 프로덕션 빌드를 생성합니다.
3. **유연성**
   - 플러그인 시스템과 다양한 프레임워크 지원.
4. **간단한 설정**
   - `vite.config.js`에서 설정을 쉽게 구성 가능.

---

## Vite 앱 사용 방법

### 1. Vite 설치 및 초기화

1. Node.js가 설치되어 있는지 확인하세요.
   ```bash
   node -v
   npm -v
   ```
2. Vite 프로젝트를 초기화합니다.
   ```bash
   npm create vite@latest
   ```
   - 원하는 프로젝트 이름을 입력합니다.
   - 사용할 프레임워크를 선택합니다(React, Vue, Svelte 등).

3. 디렉토리로 이동 후 의존성을 설치합니다.
   ```bash
   cd 프로젝트명
   npm install
   ```

4. 개발 서버를 실행합니다.
   ```bash
   npm run dev
   ```

---

### 2. 주요 스크립트

- **`npm run dev`**: 개발 서버 시작
- **`npm run build`**: 프로덕션 빌드 생성
- **`npm run preview`**: 빌드된 앱을 로컬 서버에서 미리보기

---

## 디렉토리 구조

Vite 앱의 기본 디렉토리 구조는 다음과 같습니다:

```plaintext
프로젝트명/
├── public/            # 정적 파일(빌드 과정 없이 배포 가능)
│   └── favicon.ico
├── src/               # 소스 코드 디렉토리
│   ├── assets/        # 이미지, 스타일 파일 등
│   ├── components/    # 재사용 가능한 컴포넌트
│   ├── App.jsx        # 메인 애플리케이션 컴포넌트
│   ├── main.jsx       # 진입 파일(React/Vue 초기화)
│   └── styles/        # 스타일 파일
├── index.html         # 기본 HTML 파일
├── package.json       # 의존성 및 npm 스크립트 정의
├── vite.config.js     # Vite 설정 파일
└── README.md          # 프로젝트 설명
```

---

## 앱 구조

Vite는 프레임워크에 따라 기본 템플릿 구조가 다릅니다. 여기서는 React 기반 Vite 앱의 구조를 설명합니다.

### 1. `main.jsx` (진입 파일)

애플리케이션의 시작점으로 ReactDOM을 사용하여 컴포넌트를 렌더링합니다.

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './styles/index.css';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

### 2. `App.jsx` (메인 컴포넌트)

React 컴포넌트를 작성하여 UI를 정의합니다.

```jsx
import React from 'react';

function App() {
  return (
    <div>
      <h1>Welcome to Vite + React!</h1>
    </div>
  );
}

export default App;
```

### 3. `vite.config.js` (설정 파일)

Vite의 설정을 사용자 정의할 수 있는 파일입니다.

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  server: {
    port: 3000, // 개발 서버 포트
  },
  build: {
    outDir: 'dist', // 빌드 결과 디렉토리
  },
});
```

---

## Vite 설정 및 디렉토리 확장

### 1. **Alias 설정**

`@`를 사용하여 `src` 디렉토리를 참조할 수 있도록 설정:

```javascript
export default defineConfig({
  resolve: {
    alias: {
      '@': '/src',
    },
  },
});
```

### 2. **환경 변수 관리**

Vite는 `.env` 파일을 지원하며, `VITE_`로 시작하는 환경 변수를 사용할 수 있습니다.

```plaintext
VITE_API_URL=https://api.example.com
```

### 3. **플러그인 추가**

플러그인을 사용하여 기능을 확장합니다.

- 예: SVG 파일을 컴포넌트로 사용
  ```bash
  npm install vite-plugin-svgr
  ```
  ```javascript
  import svgr from 'vite-plugin-svgr';

  export default defineConfig({
    plugins: [react(), svgr()],
  });
  ```

---

## Vite 사용 시 주의점

1. **모듈 호환성**
   - 일부 CommonJS 기반 패키지는 추가 설정이 필요할 수 있습니다.
2. **빌드 최적화**
   - 프로젝트가 커질수록 Vite의 `build.optimizeDeps` 설정을 활용하여 최적화를 고려해야 합니다.
3. **서버 사이드 렌더링(SSR)**
   - SSR이 필요한 경우 Vite의 [SSR 가이드](https://vitejs.dev/guide/ssr.html)를 참조하세요.

---

Vite는 빠르고 간단한 개발 환경을 제공하며, 다양한 프로젝트에서 생산성을 크게 향상시킬 수 있는 도구입니다. 추가적으로 특정 기능이나 디렉토리 구조 확장이 필요하면 댓글로 알려주세요! 😊

