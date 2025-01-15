---
title: "React 개발: Create React App(CRA) vs Vite, 무엇을 선택할까?"
excerpt: "React 개발을 시작할 때 Create React App과 Vite의 차이점, 장단점, 그리고 대안들에 대해 상세히 설명합니다."
categories:
  - React
  - Frontend
tags:
  - [React, CRA, Vite, Frontend, 개발 도구]
permalink: /react/cra-vs-vite/
toc: true
toc_sticky: true
date: 2025-01-14
last_modified_at: 2025-01-14
---

# React 개발: Create React App(CRA) vs Vite, 무엇을 선택할까?
React 애플리케이션 개발을 시작할 때, 어떤 도구를 선택해야 할지 고민되곤 합니다. 이 글에서는 **Create React App (CRA)**와 **Vite**의 차이점, 장단점, 그리고 대안들에 대해 자세히 살펴보겠습니다.

---

## 1. Create React App (CRA)

**CRA**는 React 팀에서 공식적으로 제공하는 도구로, React 애플리케이션 설정을 간소화하는 데 초점을 맞춥니다.

### 특징
- **Webpack** 기반의 번들링 시스템.
- 기본적으로 ESLint, Babel, Fast Refresh 등의 설정 제공.
- `react-scripts`로 빌드와 개발 서버 실행.

### 장점
1. **안정성과 광범위한 사용성**: 공식 도구로 커뮤니티 지원과 자료가 풍부.
2. **완전한 설정 제공**: 설정이 미리 구성되어 있어 초보자도 쉽게 시작 가능.
3. **확장 가능**: `eject` 명령으로 숨겨진 설정을 드러내어 커스터마이징 가능.

### 단점
1. **느린 빌드 속도**: 웹팩 기반으로 초기 로딩과 변경 사항 반영 속도가 느림.
2. **무거운 설정**: 불필요한 설정이 포함될 가능성.
3. **한정된 커스터마이징**: 기본적으로 숨겨진 설정으로 인해 제약이 있음.

```bash
# CRA로 프로젝트 생성
npx create-react-app my-app
cd my-app
npm start
```

---

## 2. Vite

**Vite**는 경량화와 빠른 개발 환경을 목표로 하는 최신 빌드 도구입니다.

### 특징
- **ES 모듈 기반**으로 개발 서버에서 즉시 로드.
- **Rollup 기반** 번들러를 사용하여 빠른 빌드 제공.
- React, TypeScript 등 다양한 프레임워크에 대한 기본 지원 제공.

### 장점
1. **빠른 속도**: 개발 서버 시작과 변경 사항 반영이 매우 빠름.
2. **경량화**: 불필요한 설정 없이 최소한의 구성 제공.
3. **유연성**: 다양한 플러그인과 Rollup 생태계를 활용 가능.
4. **현대적인 설계**: 최신 브라우저와 표준 기술 지원.

### 단점
1. **신뢰성**: CRA에 비해 커뮤니티 자료와 문서가 적음.
2. **러닝 커브**: 플러그인 설정에 대한 추가 학습 필요.
3. **레거시 지원 한계**: 오래된 브라우저를 지원하려면 추가 설정 필요.

```bash
# Vite로 React 프로젝트 생성
npm create vite@latest my-app --template react
cd my-app
npm install
npm run dev
```

---

## 3. CRA vs. Vite 비교

| 항목               | CRA                              | Vite                         |
|--------------------|----------------------------------|------------------------------|
| **빌드 속도**       | 느림                              | 매우 빠름                     |
| **개발 서버 속도**   | 느림                              | 매우 빠름                     |
| **설정 간소화**      | 설정이 숨겨져 있음                 | 간단하고 플러그인 활용 가능       |
| **커스터마이징**     | 제한적 (eject 필요)               | 매우 유연                      |
| **생태계 및 지원**   | 안정적이고 풍부                   | 성장 중, 점점 더 널리 사용됨       |
| **레거시 브라우저 지원** | 기본 제공                          | 추가 설정 필요                  |

---

## 4. CRA와 Vite 외 대안

### 4.1 Next.js
- **SEO**와 서버사이드 렌더링(SSR)에 최적화.
- 정적 사이트 생성(SSG) 및 풀스택 개발 가능.
- React 애플리케이션의 고급 기능 제공.

```bash
# Next.js로 프로젝트 생성
npx create-next-app@latest my-app
cd my-app
npm run dev
```

### 4.2 Parcel
- 설정이 거의 필요 없는 번들러.
- 빠르고 간단한 사용 가능.
- 현대적인 개발 환경 제공.

```bash
# Parcel 설치 및 실행
npm install -D parcel
parcel index.html
```

### 4.3 Webpack + Custom Config
- 높은 유연성과 세부 설정 가능.
- 대규모 프로젝트나 복잡한 요구사항에 적합.

```bash
# Webpack 설치
npm install webpack webpack-cli webpack-dev-server --save-dev
```

### 4.4 Snowpack
- ES 모듈 기반의 빠른 빌드.
- Vite와 유사하지만, 현재 사용 빈도가 감소.

```bash
# Snowpack 설치
npx create-snowpack-app my-app --template @snowpack/app-template-react
```

---

## 5. 추천
- **초보자 또는 단순한 프로젝트**: CRA.
- **빠른 개발 환경과 최신 기술 활용**: Vite.
- **SEO가 중요한 프로젝트**: Next.js.
- **복잡한 빌드 요구사항**: Webpack.

당신의 프로젝트 요구사항에 맞는 도구를 선택하여 생산성을 극대화해보세요! 😊

