---
title: "Node.js와 Express로 Mapbox Geocoding 서비스 통합하기"
excerpt: "Mapbox Geocoding API를 사용해 위치 데이터를 처리하는 방법과 Express로 이를 구현하는 과정을 알아봅니다."
categories:
  - Node
  - Express
  - Mapbox
  - Web Development
tags:
  - [Node.js, Express, Mapbox, Backend, Geocoding, API Integration]
permalink: /node/mapbox-geocoding-integration/
toc: true
toc_sticky: true
date: 2025-01-09
last_modified_at: 2025-01-09
---

## Mapbox Geocoding 서비스란?
Mapbox의 Geocoding API는 주소를 좌표로 변환하거나 좌표를 주소로 변환하는 강력한 도구입니다. 이를 통해 사용자는 다양한 위치 기반 서비스를 구현할 수 있습니다. 이 글에서는 Node.js와 Express를 사용해 Mapbox Geocoding API를 통합하는 방법을 단계별로 설명합니다.

---

## 프로젝트 준비 단계

### 1. Mapbox API 키 발급
1. [Mapbox 계정](https://www.mapbox.com/)을 생성하거나 로그인합니다.
2. API 키(Access Token)를 생성합니다. **Account > Access Tokens**에서 확인할 수 있습니다.

### 2. Node.js 프로젝트 초기화
프로젝트 디렉토리를 생성하고 초기화합니다:
```bash
mkdir mapbox-geocoding-app
cd mapbox-geocoding-app
npm init -y
```

### 3. 필수 패키지 설치
Express 서버와 Mapbox SDK를 설치합니다:
```bash
npm install express mapbox-sdk dotenv
```

---

## Mapbox Geocoding API 통합하기

### 1. `.env` 파일 생성
프로젝트 루트에 `.env` 파일을 생성하고, Mapbox API 키를 저장합니다:
```
MAPBOX_ACCESS_TOKEN=your_mapbox_access_token
```

### 2. Express 서버 구현
다음은 Mapbox Geocoding 서비스를 통합한 Express 서버의 코드입니다:

```javascript
require('dotenv').config();
const express = require('express');
const mbxGeocoding = require('@mapbox/mapbox-sdk/services/geocoding');

const app = express();
const port = 3000;

// Mapbox Geocoding 클라이언트 초기화
const geocodingClient = mbxGeocoding({ accessToken: process.env.MAPBOX_ACCESS_TOKEN });

// 미들웨어 설정
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// 경로로부터 지오코딩 예제
app.get('/geocode', async (req, res) => {
  const { location } = req.query;

  if (!location) {
    return res.status(400).json({ error: 'Location parameter is required.' });
  }

  try {
    const response = await geocodingClient
      .forwardGeocode({
        query: location,
        limit: 1, // 결과 수 제한
      })
      .send();

    const features = response.body.features;

    if (features.length === 0) {
      return res.status(404).json({ error: 'No matching locations found.' });
    }

    const [result] = features;

    res.json({
      place_name: result.place_name,
      coordinates: result.geometry.coordinates, // [longitude, latitude]
    });
  } catch (error) {
    console.error('Geocoding Error:', error);
    res.status(500).json({ error: 'An error occurred during geocoding.' });
  }
});

// 서버 시작
app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
```

---

## 서버 실행 및 테스트
1. 서버를 실행합니다:
   ```bash
   node index.js
   ```
2. 브라우저나 Postman에서 다음 URL로 테스트합니다:
   ```
   http://localhost:3000/geocode?location=Seoul
   ```

### 응답 예제
```json
{
  "place_name": "Seoul, South Korea",
  "coordinates": [126.9779692, 37.566535]
}
```

---

## Mapbox 지도와 통합 (선택 사항)
지오코딩 결과를 지도에 표시하려면 프론트엔드에서 Mapbox GL JS를 사용할 수 있습니다.

### Mapbox GL JS HTML 코드 예제

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mapbox Map</title>
  <script src="https://api.mapbox.com/mapbox-gl-js/v2.15.0/mapbox-gl.js"></script>
  <link href="https://api.mapbox.com/mapbox-gl-js/v2.15.0/mapbox-gl.css" rel="stylesheet" />
  <style>
    #map { width: 100%; height: 500px; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script>
    mapboxgl.accessToken = 'your_mapbox_access_token';
    const map = new mapboxgl.Map({
      container: 'map',
      style: 'mapbox://styles/mapbox/streets-v11',
      center: [126.9779692, 37.566535], // 초기 중심 좌표
      zoom: 10
    });

    // 예시: 마커 추가
    new mapboxgl.Marker()
      .setLngLat([126.9779692, 37.566535])
      .addTo(map);
  </script>
</body>
</html>
```

---

## 마무리
Node.js와 Express를 사용해 Mapbox Geocoding 서비스를 쉽게 통합할 수 있습니다. 이를 통해 위치 기반 웹 애플리케이션을 확장하고 사용자의 경험을 향상시킬 수 있습니다.

**참고:** Mapbox API 사용량에 따라 요금제 제한이 있으니 프로젝트 개발 시 주의하세요.

