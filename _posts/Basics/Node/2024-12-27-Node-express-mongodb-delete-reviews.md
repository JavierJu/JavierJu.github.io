---
title: "Express에서 MongoDB 관계 데이터 삭제 방법: 연결된 리뷰 처리 예제"
excerpt: "Express와 MongoDB를 사용하여 캠프장 삭제 시 연결된 리뷰 데이터를 처리하는 다양한 방법을 살펴봅니다."
categories:
  - Node
tags:
  - [Node.js, Express, MongoDB, Backend, 데이터 관리]
permalink: /Node/express-mongodb-delete-reviews/
toc: true
toc_sticky: true
date: 2024-12-27
last_modified_at: 2024-12-27
---

MongoDB는 관계형 데이터베이스와 달리 외래 키 제약조건이 없습니다. 따라서 관계 데이터 삭제는 개발자가 명시적으로 처리해야 합니다. 이 글에서는 **캠프장(Campground)**을 삭제할 때 연결된 리뷰(Review) 데이터를 함께 삭제하는 여러 가지 방법을 소개합니다.

---

## 방법 1: 서버 로직에서 명시적으로 처리

삭제 라우터에서 `Campground`와 연결된 `Review` 데이터를 명시적으로 삭제하는 방식입니다.

### 구현 코드
```javascript
app.delete('/campgrounds/:id', async (req, res) => {
    const { id } = req.params;

    // 캠프장 찾기
    const campground = await Campground.findById(id);

    if (!campground) {
        return res.status(404).send('Campground not found');
    }

    // 연결된 리뷰 삭제
    await Review.deleteMany({ _id: { $in: campground.reviews } });

    // 캠프장 삭제
    await Campground.findByIdAndDelete(id);

    res.redirect('/campgrounds');
});
```

### 동작 방식
1. 삭제하려는 캠프장을 `findById`로 검색합니다.
2. `Review.deleteMany`를 사용하여 연결된 리뷰를 삭제합니다.
3. `Campground.findByIdAndDelete`로 캠프장을 삭제합니다.

### 장점
- 코드가 명시적이어서 이해하기 쉽습니다.
- 특정 삭제 로직을 필요에 따라 커스터마이징할 수 있습니다.

### 단점
- 중복 코드가 생길 가능성이 있습니다.
- 모델 로직과 분리되지 않아 관리가 어렵습니다.

---

## 방법 2: MongoDB 트랜잭션 사용

MongoDB 트랜잭션을 활용하여 캠프장 삭제와 리뷰 삭제를 하나의 원자적 작업으로 처리합니다. 트랜잭션은 데이터 정합성이 중요한 애플리케이션에서 유용합니다.

### 구현 코드
```javascript
const mongoose = require('mongoose');

app.delete('/campgrounds/:id', async (req, res) => {
    const { id } = req.params;

    const session = await mongoose.startSession();
    session.startTransaction();

    try {
        const campground = await Campground.findById(id).session(session);

        if (!campground) {
            throw new Error('Campground not found');
        }

        // 연결된 리뷰 삭제
        await Review.deleteMany({ _id: { $in: campground.reviews } }).session(session);

        // 캠프장 삭제
        await Campground.findByIdAndDelete(id).session(session);

        await session.commitTransaction();
        session.endSession();

        res.redirect('/campgrounds');
    } catch (err) {
        await session.abortTransaction();
        session.endSession();
        console.error(err);
        res.status(500).send('Error deleting campground and associated reviews');
    }
});
```

### 동작 방식
1. MongoDB 세션을 생성하고 트랜잭션을 시작합니다.
2. 트랜잭션 내에서 연결된 리뷰를 삭제한 후 캠프장을 삭제합니다.
3. 작업이 성공하면 `commitTransaction`으로 변경 사항을 확정하고, 실패하면 `abortTransaction`으로 롤백합니다.

### 장점
- 데이터 정합성을 보장합니다.
- 실패 시 작업을 안전하게 롤백할 수 있습니다.

### 단점
- MongoDB 클러스터 환경(복제 세트 또는 샤딩)이 필요할 수 있습니다.
- 상대적으로 구현이 복잡합니다.

---

## 방법 3: Mongoose 미들웨어 사용

`Mongoose`의 **`post` 미들웨어**를 사용하여 특정 모델 삭제 후 연결된 리뷰를 삭제하는 방식입니다.

### 구현 코드
```javascript
CampgroundSchema.post('findOneAndDelete', async function (doc) {
    if (doc) {
        await Review.deleteMany({ _id: { $in: doc.reviews } });
    }
});
```

### 동작 방식
1. `findOneAndDelete` 메서드가 실행된 후 해당 캠프장의 리뷰를 삭제합니다.
2. 삭제된 캠프장 문서(`doc`)에서 `reviews` 배열을 가져와 리뷰를 삭제합니다.

### 장점
- 모델 계층에서 로직을 처리하므로 라우터 코드가 간결해집니다.
- 삭제 로직을 중앙화하여 재사용성을 높일 수 있습니다.

### 단점
- 미들웨어가 자동으로 실행되기 때문에 디버깅이 어려울 수 있습니다.
- 삭제 순서를 제어하기 어렵습니다.

---

## 방법 비교

| 방법                     | 장점                                                | 단점                                                    |
|--------------------------|---------------------------------------------------|--------------------------------------------------------|
| **서버 로직에서 명시적 처리** | 명확한 로직, 쉽게 이해 가능                           | 중복 코드 발생 가능, 모델 로직과 분리되지 않음              |
| **MongoDB 트랜잭션 사용**   | 데이터 정합성 보장, 실패 시 롤백 가능                   | 복잡한 구현, 클러스터 환경 필요                           |
| **Mongoose 미들웨어 사용** | 간결한 라우터 코드, 중앙화된 삭제 로직                  | 디버깅 어려움, 삭제 순서 제어 어려움                       |

---

## 결론

프로젝트의 규모와 요구사항에 따라 적합한 방법을 선택해야 합니다.

- 단순한 삭제 로직이라면 **서버 로직에서 명시적으로 처리**하는 방법이 효율적입니다.
- 데이터 정합성이 중요하거나 복잡한 작업이 필요한 경우 **MongoDB 트랜잭션**이 적합합니다.
- 모델 계층에서 삭제 로직을 관리하려면 **Mongoose 미들웨어**를 사용할 수 있습니다.

이 글이 MongoDB 관계 데이터 삭제에 대한 이해를 돕는 데 도움이 되길 바랍니다!

