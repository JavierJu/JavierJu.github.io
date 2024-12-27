---
title: "Express에서 MongoDB를 사용한 관계 데이터 관리 예제 : 리뷰 추가 및 삭제"
excerpt: "Express와 MongoDB를 활용하여 캠프장과 리뷰 간의 관계 데이터를 관리하는 방법을 상세히 설명합니다."
categories:
  - Node
tags:
  - [Node.js, Express, MongoDB, Backend, 리뷰 관리]
permalink: /Node/express-mongodb-reviews/
toc: true
toc_sticky: true
date: 2024-12-27
last_modified_at: 2024-12-27 
---

이 글에서는 **Express**와 **MongoDB**를 활용하여 캠프장(campgrounds)과 리뷰(reviews) 간의 관계 데이터를 처리하는 방법을 다룹니다. 관계형 데이터베이스와 달리, MongoDB에서는 문서 간 관계를 직접 정의하고 관리해야 합니다. 이를 통해 리뷰 데이터를 추가 및 삭제하는 방법을 살펴보겠습니다.

---

## 리뷰 추가 (POST 요청)

### 라우터 코드
```javascript
app.post('/campgrounds/:id/reviews', validateReview, async (req, res) => {
    const campground = await Campground.findById(req.params.id); // 캠프장 문서 가져오기
    const newReview = new Review(req.body.review); // 리뷰 생성
    campground.reviews.push(newReview); // 캠프장에 리뷰 ObjectId 추가
    await newReview.save(); // 리뷰 저장
    await campground.save(); // 캠프장 문서 업데이트
    res.redirect(`/campgrounds/${campground._id}`); // 캠프장 상세 페이지로 리다이렉트
});
```

### 동작 방식
1. **`Campground.findById`**
   - 리뷰를 추가할 대상 캠프장 문서를 가져옵니다.
2. **`new Review`**
   - 클라이언트 요청에서 전달된 데이터를 기반으로 새로운 리뷰 문서를 생성합니다.
3. **`campground.reviews.push(newReview)`**
   - 생성된 리뷰 문서의 ObjectId를 캠프장의 `reviews` 배열에 추가합니다.
4. **`newReview.save()`**
   - 리뷰 데이터를 MongoDB에 저장합니다.
5. **`campground.save()`**
   - 수정된 캠프장 데이터를 저장합니다.
6. **리다이렉트**
   - 리뷰가 추가된 캠프장 상세 페이지로 리다이렉트합니다.

---

## 리뷰 삭제 (DELETE 요청)

### 라우터 코드
```javascript
app.delete('/campgrounds/:id/reviews/:reviewId', async (req, res) => {
    const { id, reviewId } = req.params;
    await Campground.findByIdAndUpdate(id, { $pull: { reviews: reviewId } }); // 캠프장 문서에서 리뷰 ObjectId 제거
    await Review.findByIdAndDelete(reviewId); // 리뷰 문서 삭제
    res.redirect(`/campgrounds/${id}`); // 캠프장 상세 페이지로 리다이렉트
});
```

### 동작 방식
1. **`$pull` 연산자**
   - `Campground` 문서의 `reviews` 배열에서 특정 리뷰의 ObjectId를 제거합니다.
2. **`Review.findByIdAndDelete`**
   - MongoDB에서 해당 리뷰 문서를 삭제합니다.
3. **리다이렉트**
   - 삭제가 완료된 캠프장 상세 페이지로 리다이렉트합니다.

---

## 캠프장 삭제 시 연결된 리뷰 삭제

캠프장을 삭제할 경우 해당 캠프장에 연결된 모든 리뷰도 함께 삭제하려면 **Mongoose 미들웨어**를 사용할 수 있습니다. 다음은 이를 구현한 코드입니다.

### Mongoose 미들웨어 코드
```javascript
CampgroundSchema.post('findOneAndDelete', async function (doc) {
    if (doc) {
        await Review.deleteMany({ _id: { $in: doc.reviews } });
    }
});
```

### 동작 방식
1. **`findOneAndDelete` 훅**
   - `findOneAndDelete` 메서드가 호출된 후에 실행됩니다.
2. **`doc` 확인**
   - 삭제된 캠프장 문서가 존재할 경우에만 실행됩니다.
3. **`Review.deleteMany`**
   - 삭제된 캠프장의 `reviews` 배열에 포함된 모든 리뷰를 삭제합니다.

이 코드를 통해 캠프장을 삭제할 때 연결된 리뷰 데이터도 자동으로 정리할 수 있습니다.

---

## 리뷰를 표시하고 삭제 버튼 제공 (EJS 템플릿)

### EJS 템플릿 코드
```html
<h2>Leave a Review</h2>
<form action="/campgrounds/<%= campground._id %>/reviews" method="post">
    <label for="rating">Rating</label>
    <input type="range" id="rating" name="review[rating]" min="0" max="5" step="1">
    
    <label for="body">Review</label>
    <textarea id="body" name="review[body]" required></textarea>
    
    <button type="submit">Submit</button>
</form>

<% for (let review of campground.reviews) { %>
    <div>
        <h5>Rating: <%= review.rating %></h5>
        <p><%= review.body %></p>
        <form action="/campgrounds/<%= campground._id %>/reviews/<%= review._id %>?_method=DELETE" method="post">
            <button type="submit">Delete</button>
        </form>
    </div>
<% } %>
```

### 동작 방식
1. **리뷰 추가 폼**
   - `POST /campgrounds/:id/reviews` 요청을 통해 리뷰 데이터를 서버로 전달합니다.
2. **리뷰 표시**
   - `campground.reviews` 배열을 반복하여 각 리뷰 데이터를 표시합니다.
3. **삭제 버튼**
   - 각 리뷰 항목에 `DELETE` 요청을 보내는 버튼을 제공합니다.

---

## 데이터 관계 모델링

### Campground 모델
```javascript
const CampgroundSchema = new mongoose.Schema({
    name: String,
    reviews: [
        {
            type: mongoose.Schema.Types.ObjectId,
            ref: 'Review'
        }
    ]
});
```

### Review 모델
```javascript
const ReviewSchema = new mongoose.Schema({
    rating: Number,
    body: String
});
```

- `Campground`는 리뷰 문서들의 ObjectId 배열(`reviews`)을 통해 `Review`와 1:N 관계를 가집니다.

---

## 주의사항 및 팁
- **트랜잭션 사용**
  - 리뷰 추가 및 삭제는 두 개의 문서를 다루므로, 데이터 정합성을 보장하려면 MongoDB 트랜잭션을 활용할 수 있습니다.
- **미들웨어**
  - `validateReview`와 같은 미들웨어를 활용하여 리뷰 데이터의 유효성을 검사합니다.
- **에러 핸들링**
  - 데이터베이스 요청 실패에 대비한 에러 처리 로직을 추가하면 더욱 안정적인 애플리케이션을 구현할 수 있습니다.

---

이 예제는 MongoDB의 문서 간 관계를 다루는 기본적인 방법을 보여줍니다. 이 패턴은 확장성이 뛰어나며, 더 복잡한 애플리케이션 구조에도 응용할 수 있습니다.

