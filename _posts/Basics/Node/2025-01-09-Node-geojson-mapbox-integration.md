---
title: "GeoJSON과 Mapbox Geocoding 연동: Node.js와 MongoDB로 구현하기"
excerpt: "GeoJSON 데이터 구조와 Mapbox의 Geocoding API를 활용하여 Node.js와 MongoDB로 지리 데이터를 저장하고 활용하는 방법을 알아봅니다."
categories:
  - Web Development
  - GeoSpatial
  - Node.js
  - MongoDB
  - Mapbox
  
tags:
  - [GeoJSON, Mapbox, Geocoding, Node.js, MongoDB, Express]
permalink: /node/geojson-mapbox-integration/
toc: true
toc_sticky: true
date: 2025-01-09
last_modified_at: 2025-01-09
---

## GeoJSON이란?
GeoJSON은 JSON 형식을 기반으로 하는 지리적 데이터를 표현하기 위한 표준 포맷입니다. GeoJSON은 점, 선, 다각형 같은 지리적 특징을 나타내며, 각 특징은 속성 데이터와 함께 표현됩니다. 이를 통해 지도 데이터와 관련된 애플리케이션에서 데이터를 저장하고 교환하는 데 유용합니다.

### 주요 GeoJSON 객체
#### 1. Point (점)
```json
{
  "type": "Point",
  "coordinates": [longitude, latitude]
}
```
#### 2. LineString (선)
```json
{
  "type": "LineString",
  "coordinates": [
    [longitude1, latitude1],
    [longitude2, latitude2]
  ]
}
```
#### 3. Polygon (다각형)
```json
{
  "type": "Polygon",
  "coordinates": [
    [
      [longitude1, latitude1],
      [longitude2, latitude2],
      [longitude3, latitude3],
      [longitude1, latitude1]
    ]
  ]
}
```
#### 4. Feature (속성 포함 객체)
```json
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [longitude, latitude]
  },
  "properties": {
    "name": "Example Location"
  }
}
```
#### 5. FeatureCollection (여러 Feature 모음)
```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [longitude, latitude]
      },
      "properties": {
        "name": "Example Location 1"
      }
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [longitude, latitude]
      },
      "properties": {
        "name": "Example Location 2"
      }
    }
  ]
}
```

---

## Mapbox의 Geocoding API와 연동하기

Mapbox는 주소를 좌표로 변환(지오코딩)하거나 좌표를 주소로 변환(역지오코딩)하는 Geocoding API를 제공합니다. 이를 활용해 GeoJSON 데이터를 생성하고 MongoDB에 저장할 수 있습니다.

### 1. Mapbox API 사용 단계
#### 1.1 Mapbox Access Token 발급
Mapbox 계정에서 Access Token을 발급받습니다.

#### 1.2 API 엔드포인트
- **Forward Geocoding** (주소 → 좌표 변환):
  ```bash
  https://api.mapbox.com/geocoding/v5/mapbox.places/{query}.json?access_token=YOUR_ACCESS_TOKEN
  ```
- **Reverse Geocoding** (좌표 → 주소 변환):
  ```bash
  https://api.mapbox.com/geocoding/v5/mapbox.places/{longitude},{latitude}.json?access_token=YOUR_ACCESS_TOKEN
  ```

#### 1.3 Node.js에서 API 호출
`axios` 또는 `node-fetch`를 사용해 API를 호출할 수 있습니다.

### 2. MongoDB와 GeoJSON 저장
MongoDB의 `2dsphere` 인덱스를 사용하면 GeoJSON 데이터를 효율적으로 저장하고 쿼리할 수 있습니다.

---

## Node.js와 MongoDB 통합 구현

### 설치 및 준비
```bash
npm install axios mongoose express body-parser
```

### 코드 예제
```javascript
const express = require('express');
const mongoose = require('mongoose');
const axios = require('axios');

const app = express();
app.use(express.json());

// MongoDB 스키마 정의
const locationSchema = new mongoose.Schema({
  name: String,
  location: {
    type: {
      type: String,
      enum: ['Point'], // GeoJSON 타입
      required: true,
    },
    coordinates: {
      type: [Number], // [경도, 위도]
      required: true,
    },
  },
});
locationSchema.index({ location: '2dsphere' }); // 2dsphere 인덱스 생성
const Location = mongoose.model('Location', locationSchema);

// Mapbox API Key
const MAPBOX_API_KEY = 'your_mapbox_token';

// Geocoding 및 데이터 저장
app.post('/add-location', async (req, res) => {
  const { address, name } = req.body;

  try {
    // Geocoding 요청
    const geoResponse = await axios.get(
      `https://api.mapbox.com/geocoding/v5/mapbox.places/${encodeURIComponent(address)}.json?access_token=${MAPBOX_API_KEY}`
    );
    const [longitude, latitude] = geoResponse.data.features[0].center;

    // MongoDB에 저장
    const newLocation = new Location({
      name,
      location: {
        type: 'Point',
        coordinates: [longitude, latitude],
      },
    });
    await newLocation.save();

    res.status(201).json({ message: 'Location added successfully', newLocation });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// 데이터 검색 (근처 위치 찾기)
app.get('/nearby', async (req, res) => {
  const { longitude, latitude, distance } = req.query;

  try {
    const locations = await Location.find({
      location: {
        $near: {
          $geometry: {
            type: 'Point',
            coordinates: [parseFloat(longitude), parseFloat(latitude)],
          },
          $maxDistance: parseInt(distance), // 미터 단위
        },
      },
    });

    res.json(locations);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// 서버 실행
mongoose
  .connect('mongodb://localhost:27017/geo-demo', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => app.listen(3000, () => console.log('Server is running on port 3000')))
  .catch((error) => console.error(error));
```

---

## 주요 포인트
1. **GeoJSON 데이터 구조**: MongoDB의 `2dsphere` 인덱스를 활용해 지리 데이터를 저장하고 검색.
2. **Mapbox Geocoding API 연동**: 주소 데이터를 좌표로 변환 후 저장.
3. **지리적 쿼리 활용**: `$near`, `$geoWithin` 등을 통해 근처 데이터를 검색.

위 코드와 방법을 활용해 지리적 데이터를 다루는 프로젝트를 손쉽게 구현할 수 있습니다.

