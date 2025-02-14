---
title: "data-src vs src: 차이점과 Lazy Loading 최적화 방법"
excerpt: "`data-src`와 `src` 속성의 차이를 알아보고, Lazy Loading을 활용한 이미지 최적화 방법을 코드 예제와 함께 설명합니다."
categories:
  - Web Performance
  - Frontend
  - HTML
  - Web
tags:
  - [HTML, Lazy Loading, Web Performance, SEO, 최적화]
permalink: /web/data-src-vs-src/
toc: true
toc_sticky: true
date: 2025-02-11
last_modified_at: 2025-02-11
---

웹사이트에서 이미지 로딩 속도는 사용자 경험(UX)과 SEO에 큰 영향을 미칩니다.  
특히, **Lazy Loading(지연 로딩)**을 활용하면 초기 페이지 로딩 속도를 개선할 수 있습니다.  
이 글에서는 `data-src`와 `src`의 차이점을 비교하고, Lazy Loading을 활용한 최적화 방법을 설명하겠습니다.  

---

## 1️⃣ `src` 속성이란?  

`src`는 `img`, `iframe` 같은 태그에서 **파일 경로를 지정하는 기본 속성**입니다.  
브라우저는 `src` 속성이 포함된 요소를 발견하면 **즉시 해당 파일을 다운로드**하여 화면에 표시합니다.  

### ✅ `src` 사용 예제  

```html
<img src="image.jpg" alt="Example Image">
```
✔ 브라우저가 image.jpg를 즉시 로드하여 표시합니다.

## 2️⃣ `data-src` 속성이란?
`data-src`는 HTML5의 `data-*` 속성 중 하나로, **이미지 로딩을 제어하기 위해 사용됩니다.**
보통 **Lazy Loading을 구현할 때 활용**하며, 초기에는 `src`가 아닌 `data-src`에 이미지 경로를 저장합니다.
이후 JavaScript가 `data-src` 값을 `src`로 변경하여 이미지가 로드되도록 만듭니다.

### ✅ `data-src` 사용 예제 (Lazy Loading)

```html
<img data-src="image.jpg" src="placeholder.jpg" alt="Example Image">
```
✔ 초기에는 `placeholder.jpg`가 로드됨.
✔ 스크롤 시 JavaScript가 `data-src` 값을 `src`로 변경하여 `image.jpg`가 로드됨.

## 3️⃣ `data-src`를 활용한 Lazy Loading 구현
브라우저가 기본적으로 지원하는 Lazy Loading(`loading="lazy"`)을 사용하지 못하는 경우,
다음과 같이 `IntersectionObserver`를 활용하여 `data-src`를 `src`로 변경할 수 있습니다.

### ✅ JavaScript로 Lazy Loading 구현

```Javascript
document.addEventListener("DOMContentLoaded", function () {
  const images = document.querySelectorAll("img[data-src]");

  function preloadImage(img) {
    const src = img.getAttribute("data-src");
    if (!src) return;
    img.src = src;
    img.removeAttribute("data-src"); // data-src 속성 제거
  }

  const imgObserver = new IntersectionObserver((entries, observer) => {
    entries.forEach(entry => {
      if (!entry.isIntersecting) return;
      preloadImage(entry.target);
      observer.unobserve(entry.target);
    });
  });

  images.forEach(image => {
    imgObserver.observe(image);
  });
});
```

**✔ 스크롤하여 이미지가 화면에 나타나면 `data-src` 값을 `src`로 변경하여 로드**

**✔ 이미지가 로드된 후 `data-src` 속성을 제거하여 더 이상 중복 요청되지 않도록 함**

## 4️⃣ `loading="lazy"` vs `data-src`
최신 브라우저에서는 `loading="lazy"` 속성을 지원하여,
추가적인 JavaScript 없이도 Lazy Loading을 쉽게 적용할 수 있습니다.

### ✅ `loading="lazy"` 사용 예제 (권장)

```html
<img src="image.jpg" loading="lazy" alt="Example Image">
```
**✔ 브라우저가 자동으로 Lazy Loading을 적용하여 스크롤될 때 이미지 로드**


🔹 `loading="lazy"`와 `data-src` 차이점

| **비교 항목**	| **loading="lazy"** | **data-src** |
|-----------|----------------|----------|
**추가 JavaScript 필요 여부**	| ❌ 필요 없음 (브라우저 기본 지원) |	✅ 필요함 (스크롤 감지 후 `src` 변경) |
**SEO 친화적**	| ✅ 네 (검색 엔진이 `src`를 인식)	| ❌ 아니오 (JavaScript로 로드해야 검색됨) |
**브라우저 지원 여부**	| ✅ 최신 브라우저에서 기본 지원	| ✅ 모든 브라우저에서 가능 |
**초기 로딩 속도**	| ✅ 빠름	| ✅ 더 빠름 (초기에는 `placeholder`만 로드) |
**동적 이미지 변경**	| ❌ 불가능	| ✅ 가능 (`data-src` 값 변경 가능) |

### ✅ 최신 브라우저에서는 `loading="lazy"`를 사용하고, 구형 브라우저 지원이 필요하면 `data-src` 방식도 고려하는 것이 좋습니다.

## 5️⃣ 결론: 언제 `src`와 `data-src`를 사용해야 할까?
### ✅ 일반적인 경우 (`src` 사용)
👉 Lazy Loading이 필요 없으면 **그냥 `src` 속성만 사용하면 됨**

```html
<img src="image.jpg" alt="Example Image">
```

### ✅ Lazy Loading을 적용하려면 (`loading="lazy"` 사용)
👉 추가적인 JavaScript 없이 **브라우저가 자동으로 처리**

```html
<img src="image.jpg" loading="lazy" alt="Example Image">
```

### ✅ 더 정교한 Lazy Loading이 필요하면 (`data-src` 사용)
👉 JavaScript를 사용하여 **스크롤될 때 `src`를 변경**

```html
<img data-src="image.jpg" src="placeholder.jpg" alt="Example Image">
```

```js
document.addEventListener("DOMContentLoaded", function () {
  const images = document.querySelectorAll("img[data-src]");
  images.forEach(img => img.src = img.getAttribute("data-src"));
});
```

## 🚀 마무리
- `src`는 **기본적인 이미지 로딩 속성**으로, 브라우저가 즉시 로드함.

- `data-src`는 **Lazy Loading을 위한 속성**으로, JavaScript를 활용해 동적으로 이미지를 로드할 수 있음.

- 최신 브라우저에서는 `loading="lazy"`를 사용하면 **추가적인 JavaScript 없이 Lazy Loading이 가능**.

- **구형 브라우저 지원이 필요하다면 `data-src` + JavaScript 방식도 고려할 수 있음.**

**👉 웹사이트의 성능 최적화를 위해, 최신 브라우저에서는 `loading="lazy"`를 적극 활용하고,
필요한 경우 `data-src`와 JavaScript를 함께 사용하여 최적의 성능을 달성하세요! 🚀**