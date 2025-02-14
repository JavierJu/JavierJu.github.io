---
title: "Google Lighthouse: 웹사이트 성능과 SEO 최적화를 위한 필수 도구"
excerpt: "Google Lighthouse는 웹사이트의 성능, 접근성, SEO, 보안 및 PWA 지원 여부를 분석하는 강력한 오픈 소스 도구입니다. 이 글에서는 Lighthouse의 기능과 사용법을 자세히 알아보고, 성능 최적화 방법을 코드 예제와 함께 설명합니다."
categories:
  - Web
  - Performance Optimization
  - SEO
  - Google Tools
tags:
  - [Google Lighthouse, 웹 성능, SEO, 웹 최적화, PWA]
permalink: /web/lighthouse-guide/
toc: true
toc_sticky: true
date: 2025-02-12
last_modified_at: 2025-02-12
---

## **Google Lighthouse란?**

**Google Lighthouse**는 웹 페이지의 성능을 자동으로 분석하고 개선 방법을 제안하는 오픈 소스 도구입니다. Chrome DevTools, Node.js CLI, 또는 Google PageSpeed Insights를 통해 실행할 수 있으며, 웹사이트의 **성능(Performance), 접근성(Accessibility), 모범 사례(Best Practices), SEO, PWA(Progressive Web App)** 등 여러 측면을 평가합니다.

## **1. Lighthouse 주요 기능**

### **① 성능 (Performance)**
웹 페이지의 로딩 속도를 측정하고 개선할 수 있도록 도와줍니다.

#### **주요 측정 항목:**
- **First Contentful Paint (FCP):** 첫 번째 콘텐츠가 화면에 표시되는 시간
- **Largest Contentful Paint (LCP):** 가장 큰 콘텐츠(보통 주요 이미지 또는 제목)가 표시되는 시간
- **Total Blocking Time (TBT):** 페이지가 로딩될 때 사용자 입력이 차단된 총 시간
- **Cumulative Layout Shift (CLS):** 예상치 못한 레이아웃 변경 정도 (화면 안정성 평가)

### **② 접근성 (Accessibility)**
웹사이트가 장애인을 포함한 모든 사용자가 접근하기 적절한지 검사합니다.

#### **주요 체크 항목:**
- 색상 대비가 적절한가?
- 이미지에 적절한 `alt` 속성이 있는가?
- 키보드 네비게이션이 가능한가?
- `aria-label` 및 `role` 속성이 올바르게 적용되었는가?

### **③ 모범 사례 (Best Practices)**
보안 및 최신 웹 기술이 적절히 사용되고 있는지 확인합니다.

#### **검사 항목:**
- HTTPS를 사용하고 있는가?
- 최신 JavaScript 및 보안 헤더가 적용되었는가?
- 브라우저 콘솔에 오류가 발생하는가?

### **④ SEO (검색 엔진 최적화)**
웹사이트가 검색엔진에서 잘 노출될 수 있도록 SEO 점검을 수행합니다.

#### **SEO 최적화 항목:**
- 적절한 `meta title` 및 `meta description` 태그 적용 여부
- `alt` 태그를 활용하여 이미지 검색 최적화
- 모바일 친화적인 디자인 적용 여부

### **⑤ 프로그레시브 웹 앱 (PWA)**
웹 애플리케이션이 PWA로 동작하는지 분석합니다.

#### **검사 항목:**
- 서비스 워커(Service Worker) 적용 여부
- 적절한 `manifest.json` 설정 여부
- 오프라인 지원 여부

## **2. Lighthouse 사용 방법**

### **① Chrome DevTools에서 실행**
1. Chrome 브라우저에서 분석할 웹사이트를 엽니다.
2. `F12` 또는 `Ctrl + Shift + I` (`Cmd + Option + I` for Mac)로 개발자 도구(DevTools)를 엽니다.
3. `Lighthouse` 탭을 선택합니다.
4. 분석할 항목을 선택한 후 "Analyze page load"를 클릭합니다.

### **② Chrome 확장 프로그램 사용**
1. [Lighthouse 확장 프로그램](https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk) 설치
2. 확장 프로그램 실행 후 "Generate report" 클릭

### **③ Node.js CLI에서 실행**

```sh
npm install -g lighthouse
lighthouse https://example.com --view
```

### **④ Google PageSpeed Insights 이용**
[Lighthouse 기반의 Google PageSpeed Insights](https://pagespeed.web.dev/)를 통해 온라인에서 분석 가능

## **3. Lighthouse 점수 해석 방법**
Lighthouse는 각 항목을 **0~100점**으로 평가합니다.
- **90~100:** 우수 (Good)
- **50~89:** 개선 필요 (Needs Improvement)
- **0~49:** 낮음 (Poor)

점수는 실행할 때마다 변동될 수 있으며, 네트워크 상태 및 환경에 따라 차이가 발생할 수 있습니다.

## **4. Lighthouse 점수 개선 방법**

### 🚀 **성능(Performance) 개선 방법**
```html
<!-- Lazy Loading 적용 -->
<img src="example.jpg" loading="lazy" alt="이미지 설명">
```
- 이미지 최적화 (`WebP` 사용, 크기 조절)
- 캐싱 (`Cache-Control`, `ETag` 설정)
- 불필요한 JavaScript 최소화 및 코드 분할 (Code Splitting)

### ♿ **접근성(Accessibility) 개선**
```html
<!-- 올바른 ARIA 속성 사용 -->
<button aria-label="닫기">X</button>
```
- 색 대비 기준 충족 (contrast ratio ≥ 4.5:1)
- `alt` 속성 추가

### 🔒 **모범 사례(Best Practices) 개선**
```sh
# HTTPS 리디렉션 설정 (Nginx 예제)
server {
  listen 80;
  server_name example.com;
  return 301 https://$host$request_uri;
}
```
- HTTPS 적용
- 최신 보안 헤더 설정 (`Content-Security-Policy`, `X-Frame-Options` 등)

### 🔍 **SEO 최적화**
```html
<!-- 적절한 메타 태그 적용 -->
<meta name="description" content="웹사이트 설명">
```
- `meta description` 최적화
- `alt` 태그 활용
- 모바일 친화적 디자인 적용

## **5. Lighthouse의 한계 및 보완 방법**
- 점수는 실행할 때마다 변동될 수 있음
- 실제 사용자 환경과 다를 수 있음 → [Google Chrome UX Report (CrUX)](https://developer.chrome.com/docs/crux/) 활용
- 더 깊이 있는 SEO 분석이 필요하다면 [Google Search Console](https://search.google.com/search-console/)과 함께 사용

## **결론**
Google Lighthouse는 웹사이트의 **성능, 접근성, SEO, 보안 및 PWA 지원 여부**를 평가하는 필수 도구입니다. 꾸준한 최적화를 통해 웹사이트의 성능을 향상시키고, 검색엔진에서 더 높은 순위를 차지할 수 있습니다. 🚀

