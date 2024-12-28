---
title: "Express 라우터 분리와 참조: Node.js 애플리케이션 설계의 핵심"
excerpt: "Express 애플리케이션에서 라우터를 모듈화하고 사용하는 방법을 살펴보며, 유지 보수성과 확장성을 극대화하는 설계 패턴을 소개합니다."
categories:
  - Express
  - Node
tags:
  - [Express, Node.js, Backend, 서버 개발]
permalink: /node/express-router-splitting/
toc: true
toc_sticky: true
date: 2024-12-28
last_modified_at: 2024-12-28
---

Express에서 라우터를 분리하는 것은 애플리케이션의 구조를 개선하고 유지 보수를 용이하게 만드는 중요한 설계 패턴입니다. 이 글에서는 라우터를 분리하고 사용하는 방법을 실제 코드 예제를 통해 살펴보겠습니다.

---

## 라우터 분리의 필요성

Express 애플리케이션의 규모가 커지면 모든 라우트를 한 파일에서 관리하기 어렵습니다. 이를 해결하기 위해 라우터를 모듈화하면 다음과 같은 이점이 있습니다:

- **모듈화**: 코드가 더 읽기 쉽고 관리하기 쉬워집니다.
- **책임 분리**: 서로 다른 URL 경로와 관련된 로직을 분리하여 관심사를 명확히 합니다.
- **재사용성**: 모듈화된 라우터는 다른 애플리케이션에서도 손쉽게 재사용할 수 있습니다.

---

## 코드 예제

### 1. `index.js` - 메인 애플리케이션 파일

```javascript
const express = require('express');
const app = express();
const shelterRoutes = require('./routes/shelters');
const adminRoutes = require('./routes/admin');

app.use('/shelters', shelterRoutes);
app.use('/admin', adminRoutes);

app.listen(3000, () => {
    console.log('Serving app on localhost:3000');
});
```

#### 주요 설명
- **`require`를 사용한 라우터 가져오기**: 외부 모듈로 정의된 라우터 파일을 가져옵니다.
- **`app.use`로 경로 연결**: 특정 경로(`/shelters`, `/admin`)에 라우터를 연결하여 URL 요청을 처리합니다.

---

### 2. `shelters.js` - `/shelters` 경로를 처리하는 라우터

```javascript
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
    res.send("ALL SHELTERS");
});

router.post('/', (req, res) => {
    res.send("CREATING SHELTER");
});

router.get('/:id', (req, res) => {
    res.send("VIEWING ONE SHELTER");
});

router.get('/:id/edit', (req, res) => {
    res.send("EDITING ONE SHELTER");
});

module.exports = router;
```

#### 주요 설명
- **라우터 생성**: `express.Router()`를 호출하여 라우터 객체를 생성합니다.
- **라우트 정의**: HTTP 메서드(GET, POST 등)와 경로를 지정하여 각 요청에 대한 핸들러를 정의합니다.
- **경로 매개변수**: `/:id`와 같이 동적 경로를 처리합니다.
- **라우터 내보내기**: `module.exports`로 라우터를 외부 파일에 노출합니다.

---

### 3. `admin.js` - `/admin` 경로를 처리하는 라우터

```javascript
const express = require('express');
const router = express.Router();

router.use((req, res, next) => {
    if (req.query.isAdmin) {
        return next();
    }
    return res.send("SORRY NOT AN ADMIN!");
});

router.get('/topsecret', (req, res) => {
    res.send('THIS IS TOP SECRET');
});

router.get('/deleteeverything', (req, res) => {
    res.send('OK DELETED IT ALL!');
});

module.exports = router;
```

#### 주요 설명
- **미들웨어 사용**: 요청을 처리하기 전에 `isAdmin` 쿼리 파라미터를 확인하여 권한을 제한합니다.
- **관리자 전용 경로**: `/topsecret`, `/deleteeverything` 같은 민감한 경로를 정의합니다.

---

## 라우터 분리의 결과

1. **모듈화된 코드**: 라우트가 파일별로 나뉘어 있어 읽기 쉽고 관리가 간편합니다.
2. **확장성**: 새로운 경로나 기능을 추가할 때 기존 구조에 쉽게 통합할 수 있습니다.
3. **재사용성**: 잘 설계된 라우터는 다른 프로젝트에서도 재사용 가능합니다.

---

## 개선점 및 팁

1. **미들웨어 분리**:
   - 반복적으로 사용되는 미들웨어(예: 권한 검사)는 별도 파일로 추출해 재사용성을 높입니다.

2. **폴더 구조 개선**:
   - 라우터 파일이 많아질 경우 폴더 구조를 계층적으로 설계합니다.
     ```
     /routes
       /shelters.js
       /admin.js
       /users.js
     ```

3. **에러 핸들링**:
   - 라우터에서 발생하는 에러를 처리하기 위해 중앙 집중식 에러 핸들러를 구현합니다.

---

Express에서 라우터를 분리하고 사용하는 것은 단순히 코드 정리에 그치지 않고, 유지 보수성과 확장성을 극대화하는 설계 방법입니다. 위의 예제를 활용해 더 나은 Express 애플리케이션을 만들어 보세요!

