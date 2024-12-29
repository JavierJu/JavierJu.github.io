---
title: "Express 라우터의 mergeParams: true 옵션 이해하기"
excerpt: "Express에서 라우터 간 매개변수 공유를 가능하게 하는 mergeParams: true 옵션의 필요성과 사용법을 알아봅니다."
categories:
  - Express
  - Node
tags:
  - [Express, Node.js, Backend, 서버 개발]
permalink: /node/express-mergeParams-true/
toc: true
toc_sticky: true
date: 2024-12-29
last_modified_at: 2024-12-29
---

## Express 라우터의 `mergeParams: true` 옵션 이해하기

Express에서 라우터를 설계할 때, 상위 라우터의 경로 매개변수를 하위 라우터에서 참조해야 하는 경우가 있습니다. 이때 사용하는 옵션이 바로 `mergeParams: true`입니다. 이 글에서는 `mergeParams: true`의 동작 원리와 필요성을 실제 코드 예제를 통해 살펴봅니다.

---

## 1. 기본 동작과 문제점

Express에서 `express.Router()`로 생성된 라우터는 기본적으로 **상위 라우터의 경로 매개변수**를 가져오지 않습니다. 상위 라우터에서 정의된 매개변수를 하위 라우터에서 사용하려고 하면 `req.params`가 빈 객체가 되는 문제가 발생합니다.

### 예제 코드

#### **`app.js`**
```javascript
app.use('/campgrounds/:id/reviews', reviewRoutes);
```
- 상위 라우터에서 `:id` 경로 매개변수를 정의했습니다.
- 하위 라우터인 `reviewRoutes`에서 이 값을 사용해야 하는 경우, 기본 설정만으로는 `req.params.id`에 접근할 수 없습니다.

#### **`reviews.js` (문제 발생)**
```javascript
const router = express.Router();

router.post('/', async (req, res) => {
    console.log(req.params); // 출력: {}
});
```
- 이 상태에서는 `req.params`가 빈 객체로 출력되며, `:id`에 접근할 수 없습니다.

---

## 2. `mergeParams: true`의 역할

`mergeParams: true` 옵션을 사용하면 상위 라우터의 매개변수가 하위 라우터로 병합되어 전달됩니다. 이를 통해 하위 라우터에서도 `req.params`를 통해 상위 라우터의 매개변수에 접근할 수 있습니다.

### 수정된 코드

#### **`reviews.js` (해결)**
```javascript
const router = express.Router({ mergeParams: true });

router.post('/', async (req, res) => {
    console.log(req.params); // 출력: { id: '캠프그라운드ID' }
    const campground = await Campground.findById(req.params.id);
    const newReview = new Review(req.body.review);
    campground.reviews.push(newReview);
    await newReview.save();
    await campground.save();
    res.redirect(`/campgrounds/${campground._id}`);
});

module.exports = router;
```

#### **`app.js`**
```javascript
app.use('/campgrounds/:id/reviews', reviewRoutes);
```

- `mergeParams: true`를 설정한 덕분에, 하위 라우터에서도 상위 라우터의 `:id` 매개변수에 접근할 수 있습니다.

---

## 3. `mergeParams: true`가 필요한 이유

### **매개변수 공유**
- 상위 라우터의 경로 매개변수를 하위 라우터에서 사용할 수 있도록 병합합니다.

### **계층적 라우팅**
- 라우터가 상위-하위 관계로 설계된 경우, 상위 경로의 정보를 하위에서 참조해야 할 때 유용합니다.
  - 예: `/campgrounds/:id/reviews`에서 `:id`는 특정 캠프그라운드를 나타내며, 리뷰 작성 시 이를 참조해야 합니다.

### **데이터 종속성 관리**
- 하위 경로가 상위 엔티티에 종속된 데이터를 처리할 때 필수적입니다.
  - 리뷰 데이터를 특정 캠프그라운드에 연결하거나, 사용자 데이터와 같은 상위 리소스 정보가 필요할 때 사용됩니다.

---

## 4. 언제 사용해야 할까?

`mergeParams: true`는 다음과 같은 경우에 유용합니다:

1. **상위 라우터의 매개변수를 참조해야 하는 경우**:
   - 하위 라우터에서 `req.params`를 통해 상위 라우터의 매개변수에 접근해야 할 때.

2. **복잡한 계층적 URL 구조**:
   - 예: `/users/:userId/posts/:postId/comments`와 같은 다단계 URL 구조에서 매개변수를 공유해야 할 때.

3. **모듈화된 라우터 설계**:
   - 각 라우터를 독립적으로 설계하되, 필요한 상위 매개변수를 가져와야 할 때.

---

## 5. 정리

- `mergeParams: true`는 상위 라우터의 매개변수를 하위 라우터로 병합해주는 옵션입니다.
- 이를 통해 계층적 라우팅 구조에서도 매개변수를 손쉽게 참조할 수 있습니다.
- 데이터를 종속적으로 처리하거나 복잡한 URL 구조를 관리할 때 매우 유용합니다.

Express 애플리케이션을 설계할 때 `mergeParams: true`를 적절히 활용하면 더 명확하고 유지 보수하기 쉬운 코드를 작성할 수 있습니다.

