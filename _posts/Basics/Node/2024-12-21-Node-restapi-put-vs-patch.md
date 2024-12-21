---
title: "PUT과 PATCH의 차이점: Express와 MongoDB에서의 활용"
excerpt: "PUT과 PATCH의 차이점을 이해하고, Express와 Mongoose를 활용한 업데이트 로직에서의 적용 사례를 알아봅니다."
categories:
  - Node
tags:
  - [REST API, PUT, PATCH, HTTP Methods, Node.js, Express, Mongoose]
permalink: /Node/restapi-put-vs-patch/
toc: true
toc_sticky: true
date: 2024-12-21
last_modified_at: 2024-12-21
---

## PUT과 PATCH의 차이점

RESTful API 설계에서, 리소스 업데이트를 위해 주로 사용되는 HTTP 메서드는 `PUT`과 `PATCH`입니다. 이 둘은 목적이 비슷하지만, 작동 방식에서 중요한 차이점이 있습니다.

### 1. PUT
- **리소스를 전체적으로 교체 (Replace)**
  - 클라이언트가 서버에 보낸 데이터가 리소스를 완전히 대체합니다.
  - 기존 필드가 요청 데이터에 포함되지 않으면, 해당 필드는 삭제됩니다.

#### 예시:
기존 데이터:
```json
{
  "title": "Camp A",
  "location": "USA"
}
```

`PUT` 요청:
```json
{
  "title": "Camp B"
}
```

결과:
```json
{
  "title": "Camp B"
}
```

### 2. PATCH
- **리소스를 부분적으로 업데이트 (Partial Update)**
  - 요청 데이터로 지정된 필드만 업데이트됩니다.
  - 기존 리소스의 나머지 필드는 변경되지 않고 유지됩니다.

#### 예시:
기존 데이터:
```json
{
  "title": "Camp A",
  "location": "USA"
}
```

`PATCH` 요청:
```json
{
  "title": "Camp B"
}
```

결과:
```json
{
  "title": "Camp B",
  "location": "USA"
}
```

## Express에서의 활용
Express로 CRUD API를 설계할 때, `PUT`과 `PATCH` 모두를 사용할 수 있습니다. 아래는 예시입니다.

### 라우트 정의
```javascript
const express = require('express');
const app = express();
const Campground = require('./models/campground'); // Mongoose 모델

// 기존 데이터 수정 폼 렌더링
app.get('/campgrounds/:id/edit', async (req, res) => {
    const campground = await Campground.findById(req.params.id);
    res.render('campgrounds/edit', { campground });
});

// PUT 요청을 통한 업데이트
app.put('/campgrounds/:id', async (req, res) => {
    const campground = await Campground.findByIdAndUpdate(req.params.id, req.body, {
        runValidators: true,
        new: true
    });
    res.redirect(`/campgrounds/${campground._id}`);
});

// PATCH 요청을 통한 업데이트
app.patch('/campgrounds/:id', async (req, res) => {
    const campground = await Campground.findByIdAndUpdate(req.params.id, req.body, {
        runValidators: true,
        new: true
    });
    res.redirect(`/campgrounds/${campground._id}`);
});
```

### EJS 폼 코드
`edit.ejs`에서 `?_method=PUT` 또는 `?_method=PATCH`를 사용해 원하는 메서드를 설정합니다.

```html
<form action="/campgrounds/<%= campground._id %>?_method=PUT" method="post">
    <label for="title">Title:</label>
    <input type="text" name="title" placeholder="title" id="title" value="<%= campground.title %>">
    <br>
    <label for="location">Location:</label>
    <input type="text" name="location" placeholder="location" id="location" value="<%= campground.location %>">
    <br>
    <button type="submit">Edit</button>
</form>
```

## 개선 포인트

1. **`PATCH`를 사용하는 이유**:
   - RESTful 관례에 따라, 일부 필드만 업데이트하려면 `PATCH`를 사용하는 것이 적합합니다.

2. **명시적 필드 매핑**:
   - 보안 상의 이유로, `req.body`를 데이터 모델에 직접 전달하기보다는 수정할 필드를 명시적으로 정의하세요.

```javascript
const { title, location } = req.body;
const campground = await Campground.findByIdAndUpdate(req.params.id, { title, location }, {
    runValidators: true,
    new: true
});
```

3. **Body Parser 설정**:
   - HTML 폼 데이터를 처리하려면 `express.urlencoded` 미들웨어를 활성화해야 합니다.

```javascript
app.use(express.urlencoded({ extended: true }));
```

## 결론
`PUT`은 리소스 전체를 교체할 때 사용하고, `PATCH`는 리소스의 일부만 업데이트할 때 사용됩니다. Mongoose의 `findByIdAndUpdate`는 두 메서드 모두 동일하게 동작하지만, 의도를 명확히 전달하려면 상황에 따라 적절한 HTTP 메서드를 선택하는 것이 중요합니다.

