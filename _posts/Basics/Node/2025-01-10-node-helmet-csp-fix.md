---
title: "Helmet.js와 CSP(Content Security Policy) 오류 해결하기"
excerpt: "Helmet.js로 CSP(Content Security Policy)를 설정할 때 발생하는 외부 리소스 차단 문제를 해결하는 방법을 다룹니다."
categories:
  - Web Development
tags:
  - [Node.js, Helmet.js, Security, CSP, 웹 개발]
permalink: /node/helmet-csp-fix/
toc: true
toc_sticky: true
date: 2025-01-10
last_modified_at: 2025-01-10
---

웹 애플리케이션을 개발할 때 보안을 강화하기 위해 Helmet.js를 사용해 Content Security Policy(CSP)를 설정할 수 있습니다. 그러나 외부 리소스를 허용하지 않으면 일부 리소스가 차단되어 페이지 로딩이 실패할 수 있습니다. 이 글에서는 Helmet.js를 사용하면서 발생하는 CSP 오류와 해결 방법을 다룹니다.

## 1. CSP 오류 예시

아래는 CSP 설정 문제로 인해 발생할 수 있는 오류의 예입니다:

```text
Refused to load the script 'https://api.mapbox.com/mapbox-gl-js/v3.9.2/mapbox-gl.js' because it violates the following Content Security Policy directive: "script-src 'self'".
Refused to load the stylesheet 'https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css' because it violates the following Content Security Policy directive: "style-src 'self'".
Refused to load the font 'https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/webfonts/fa-brands-400.woff2' because it violates the following Content Security Policy directive: "font-src 'self'".
```

이러한 오류는 CSP 설정에서 필요한 리소스의 출처를 명시적으로 허용하지 않았기 때문에 발생합니다.

---

## 2. 해결 방법: Helmet.js의 CSP 설정 수정

Helmet.js의 `contentSecurityPolicy`를 수정하여 외부 리소스를 허용하면 문제를 해결할 수 있습니다.

### 코드 예제

아래는 CSP 설정을 수정한 예제 코드입니다:

```javascript
const helmet = require("helmet");

const scriptSrcUrls = [
    "https://stackpath.bootstrapcdn.com/",
    "https://api.tiles.mapbox.com/",
    "https://api.mapbox.com/",
    "https://kit.fontawesome.com/",
    "https://cdnjs.cloudflare.com/",
    "https://cdn.jsdelivr.net",
];
const styleSrcUrls = [
    "https://kit-free.fontawesome.com/",
    "https://stackpath.bootstrapcdn.com/",
    "https://api.mapbox.com/",
    "https://api.tiles.mapbox.com/",
    "https://fonts.googleapis.com/",
    "https://use.fontawesome.com/",
    "https://cdn.jsdelivr.net", // 추가된 CDN
    "https://cdnjs.cloudflare.com/", // 추가된 CDN
];
const connectSrcUrls = [
    "https://api.mapbox.com/",
    "https://a.tiles.mapbox.com/",
    "https://b.tiles.mapbox.com/",
    "https://events.mapbox.com/",
];
const fontSrcUrls = [
    "https://cdnjs.cloudflare.com", // 폰트 출처 추가
];

app.use(
    helmet.contentSecurityPolicy({
        directives: {
            defaultSrc: [],
            connectSrc: ["'self'", ...connectSrcUrls],
            scriptSrc: ["'unsafe-inline'", "'self'", ...scriptSrcUrls],
            styleSrc: ["'self'", "'unsafe-inline'", ...styleSrcUrls],
            workerSrc: ["'self'", "blob:"],
            objectSrc: [],
            imgSrc: [
                "'self'",
                "blob:",
                "data:",
                "https://res.cloudinary.com/YOUR_CLOUDINARY_ACCOUNT/", // Cloudinary URL
                "https://images.unsplash.com/",
            ],
            fontSrc: ["'self'", ...fontSrcUrls],
        },
    })
);
```

---

## 3. 주요 변경 사항

### 3.1. Script 및 Style URL 추가
- `scriptSrcUrls`: 외부 스크립트를 허용하기 위해 사용.
- `styleSrcUrls`: 외부 스타일시트를 허용하기 위해 사용.

### 3.2. Font URL 추가
- `fontSrcUrls`: 웹폰트 로드를 허용.

### 3.3. Image Source 허용
- Cloudinary 및 Unsplash와 같은 외부 이미지 호스트를 허용.

---

## 4. 결과 확인
위 설정을 적용한 후 애플리케이션을 재시작하고, 브라우저 콘솔에서 CSP 관련 오류가 사라졌는지 확인합니다. 필요한 리소스의 URL을 추가적으로 허용하여 오류를 해결할 수 있습니다.

---

이 방법을 적용하면 외부 리소스가 정상적으로 로드되면서도 보안을 유지할 수 있습니다. Helmet.js와 CSP 설정을 통해 안전하고 강력한 웹 애플리케이션을 구축해 보세요! 😊

