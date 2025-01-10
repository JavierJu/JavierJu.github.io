---
title: "EJS에서 JavaScript로 객체 데이터를 안전하게 전달하는 방법"
excerpt: "EJS를 활용하여 서버 데이터를 클라이언트 JavaScript로 안전하게 전달하는 방법을 JSON.stringify와 JSON.parse를 통해 알아봅니다."
categories:
  - Web Development
  - JavaScript
  - Backend
  - Node.js
  - EJS
  
tags:
  - [EJS, JavaScript, Node.js, Web Development, 서버 개발]
permalink: /web/ejs-to-js-data-transfer/
toc: true
toc_sticky: true
date: 2025-01-09
last_modified_at: 2025-01-09
---

# EJS에서 JavaScript로 객체 데이터를 안전하게 전달하는 방법

웹 개발에서 서버 측 렌더링을 할 때, **EJS (Embedded JavaScript)**를 사용하여 HTML 템플릿을 렌더링하고, 클라이언트 측 JavaScript에서 이를 활용하는 경우가 많습니다. 이때, **서버에서 전달된 데이터**를 JavaScript 코드에서 안전하게 사용하는 방법에 대해 알아보겠습니다.

## 문제 상황

저는 **Node.js와 Express**를 사용하여 `campground` 객체 데이터를 **Mapbox** 지도에 전달하려고 했습니다. 그러나 EJS 템플릿에서 JavaScript로 데이터를 전달할 때, `Expression expected`와 같은 오류가 발생했습니다.

```javascript
const campground = <%- JSON.stringify(campground) %>; // 이 부분에서 오류 발생
```

이 문제를 해결하기 위해 다양한 방법을 시도했으며, 최종적으로 **`JSON.parse()`**를 사용한 방법을 통해 데이터를 성공적으로 전달했습니다.

## 해결 과정

### 1. **문제의 원인**
`<%- JSON.stringify(campground) %>`는 EJS에서 객체를 **문자열로 변환**한 후 **HTML에 삽입**합니다. 그런데 이렇게 삽입된 데이터는 **JSON 형식**으로 유효하지 않아서 JavaScript에서 처리하기 어려운 상황이 발생했습니다.

특히, `JSON.parse()`로 변환할 때 **잘못된 구문**이 있으면 오류가 발생하게 됩니다.

### 2. **해결 방법**

#### EJS에서 안전하게 데이터를 전달하려면, 다음과 같은 방법을 사용해야 합니다.

##### 1) **`JSON.stringify()`로 객체를 문자열로 변환 후 `JSON.parse()`로 변환**

```html
<script>
    const mapToken = '<%- process.env.MAPBOX_ACCESS_TOKEN %>';
    const campground = JSON.parse('<%- JSON.stringify(campground) %>'); // 문자열을 JSON 객체로 변환
</script>
```

- **`<%- JSON.stringify(campground) %>`**: EJS에서 `campground` 객체를 **JSON 형식의 문자열**로 변환합니다.
- **`JSON.parse()`**: 클라이언트에서 이 문자열을 다시 **JavaScript 객체로 변환**하여 사용할 수 있게 만듭니다.

이 방식은 **EJS 템플릿**에서 데이터를 **HTML로 렌더링**한 후, **JavaScript**에서 이를 다시 **객체로 변환**할 수 있게 도와줍니다.

### 3. **`JSON.stringify()`와 `JSON.parse()`의 중요성**

- **`JSON.stringify()`**는 JavaScript 객체를 **JSON 문자열**로 변환합니다. 이 문자열을 HTML에 삽입하면, **그 데이터가 안전하게 클라이언트로 전달**될 수 있습니다.
- **`JSON.parse()`**는 이 문자열을 다시 **JavaScript 객체로 변환**하여 코드에서 사용할 수 있게 해줍니다.

### 4. **결과**
이 방식으로 데이터를 전달하면, 이제 `campground` 객체를 JavaScript 코드에서 정상적으로 사용할 수 있습니다. 예를 들어, **Mapbox 지도**에서 마커를 추가하는 등의 작업을 할 수 있습니다.

```javascript
const mapboxgl = require('mapbox-gl');
mapboxgl.accessToken = mapToken;
const map = new mapboxgl.Map({
    container: 'map', // container ID
    style: 'mapbox://styles/mapbox/streets-v12', // style URL
    center: campground.geometry.coordinates, // [lng, lat]
    zoom: 9, // zoom level
});

const marker = new mapboxgl.Marker()
    .setLngLat(campground.geometry.coordinates)
    .addTo(map);
```

### 5. **추가적인 팁**

- **HTML 이스케이프 처리**: `<%- ... %>` 구문을 사용하여 **HTML 이스케이프 없이** 데이터를 출력해야 합니다. 이 구문은 문자열을 그대로 삽입하므로, JavaScript 코드에서 바로 사용할 수 있습니다.
- **`window` 객체나 `data-*` 속성**을 사용하여 다른 방식으로 데이터를 전달할 수도 있지만, `JSON.parse()`를 이용한 방법이 가장 직관적이고 안전한 방법입니다.

## 결론

EJS에서 데이터를 JavaScript로 안전하게 전달하려면, **`JSON.stringify()`와 `JSON.parse()`**를 적절히 활용해야 합니다. 이 방법은 **서버에서 클라이언트로 데이터를 안전하게 전달**하고, 클라이언트 측에서 **올바르게 처리**할 수 있게 도와줍니다.

이 방법을 사용하면, **Mapbox** 같은 지도 API를 사용할 때 **객체 데이터를 쉽게 전달**하고 활용할 수 있습니다.

