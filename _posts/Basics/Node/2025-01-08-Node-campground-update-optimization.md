---
title: "Express와 Mongoose: 업데이트 라우터 코드 개선"
excerpt: "Express와 Mongoose를 사용한 캠프그라운드 업데이트 코드의 동작 원리를 이해하고, 가독성과 성능을 개선한 최적화 방법을 탐구합니다."
categories:
  - Node
  - Express
  - MongoDB
  - Web Development
tags:
  - [Node.js, Express, Mongoose, Backend, 코드 최적화]
permalink: /node/campground-update-optimization/
toc: true
toc_sticky: true
date: 2025-01-08
last_modified_at: 2025-01-08
---

## 원본 코드 설명

아래 코드는 Express와 Mongoose를 사용하여 특정 캠프그라운드 데이터를 업데이트하는 기능을 구현한 예제입니다:

```javascript
module.exports.updateCampground = async (req, res) => {
    const campground = await Campground.findById(req.params.id);
    const images = req.files.map(f => ({ url: f.path, filename: f.filename })) // 배열로 만들어서 push로 추가
    campground.images.push(...images);
    await campground.save();
    await Campground.findByIdAndUpdate(req.params.id, { ...req.body.campground }, { runValidators: true, new: true });
    req.flash('success', 'Successfully updated campground!');
    res.redirect(`/campgrounds/${campground._id}`);
}
```

### 동작 설명

1. **`findById` 호출**: `req.params.id`로 지정된 캠프그라운드를 데이터베이스에서 검색합니다.
2. **이미지 추가**: 요청 파일(`req.files`)을 순회하며 Cloudinary URL과 파일명을 저장한 배열을 생성한 후, 기존 이미지 배열에 추가합니다.
3. **`save` 호출**: 추가된 이미지를 포함한 캠프그라운드 객체를 저장합니다.
4. **데이터 업데이트**: 별도의 `findByIdAndUpdate`를 호출하여 나머지 데이터를 업데이트합니다.
5. **성공 메시지 및 리디렉션**: 작업이 성공하면 성공 메시지를 설정하고, 클라이언트를 해당 캠프그라운드 페이지로 리디렉션합니다.

---

## 문제점 및 개선 방안

### 문제점

1. **중복 데이터베이스 호출**:
   - `findById`와 `findByIdAndUpdate`를 호출하여 데이터베이스 작업이 불필요하게 중복됩니다.

2. **에러 핸들링 부족**:
   - 데이터베이스 작업 중 오류가 발생해도 적절히 처리하지 않습니다.

3. **코드 가독성**:
   - 이미지 추가와 데이터 업데이트 작업이 분리되지 않아 로직이 복잡해 보입니다.

### 개선 방안

1. **데이터베이스 호출 최소화**:
   - 한 번의 호출로 이미지 추가와 데이터 업데이트를 처리합니다.

2. **에러 핸들링 추가**:
   - `try-catch` 블록을 사용하여 예외 상황을 처리합니다.

3. **코드 구조 개선**:
   - 기능별로 코드 블록을 분리하여 가독성을 향상시킵니다.

---

## 개선된 코드

```javascript
module.exports.updateCampground = async (req, res) => {
    try {
        const { id } = req.params;
        const { campground: updatedData } = req.body;

        // 이미지 추가
        const images = req.files.map(f => ({ url: f.path, filename: f.filename }));

        // 데이터베이스 업데이트 (이미지 추가와 데이터 병합)
        const campground = await Campground.findByIdAndUpdate(
            id,
            { $push: { images }, ...updatedData },
            { runValidators: true, new: true }
        );

        // 성공 메시지 및 리디렉션
        req.flash('success', 'Successfully updated campground!');
        res.redirect(`/campgrounds/${campground._id}`);
    } catch (error) {
        console.error(error);
        req.flash('error', 'Failed to update campground.');
        res.redirect('/campgrounds');
    }
};
```

---

## 개선 내용

1. **중복 제거**:
   - `findById` 호출을 제거하고, `findByIdAndUpdate`에서 이미지 추가와 데이터 업데이트를 병합 처리했습니다.

2. **에러 처리**:
   - `try-catch` 블록으로 예외 상황을 처리하여 안정성을 높였습니다.

3. **가독성 향상**:
   - 변수와 로직을 목적별로 분리하여 코드의 구조를 명확히 했습니다.

---

## 결론

이 코드는 Mongoose를 사용한 데이터베이스 업데이트 작업을 최적화한 예제입니다. 중복된 호출을 제거하고 에러 처리를 추가함으로써 성능과 안정성을 모두 개선할 수 있습니다. 이와 같은 개선은 코드의 유지보수성과 확장성을 높이는 데 크게 기여합니다.

