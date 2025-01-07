---
title: "Express 라우트 리팩토링: Controller 분리와 router.route 활용"
excerpt: "Express 애플리케이션에서 라우트를 컨트롤러로 분리하고 router.route를 활용하여 가독성과 유지보수성을 개선하는 방법을 소개합니다."
categories:
  - Node
  - Express
  - Backend
  - Code Refactoring
tags:
  - [JavaScript, Node.js, Express, Backend, Refactoring]
permalink: /node/express-route-refactoring/
toc: true
toc_sticky: true
date: 2025-01-06
last_modified_at: 2025-01-06
---

Express 애플리케이션의 라우트 코드를 리팩토링하여 컨트롤러와 라우트를 분리하고, `router.route`를 활용해 코드를 간결하고 유지보수하기 쉽게 만드는 방법을 알아보겠습니다. 기존 코드와 리팩토링된 코드를 비교하며 설명합니다.

## 기존 코드의 문제점

기존 라우트 파일은 핸들러 함수가 라우트 정의와 함께 작성되어 있어 다음과 같은 문제를 초래합니다:

- **코드 중복**: 라우트 정의마다 핸들러를 포함하여 코드가 길고 복잡해집니다.
- **가독성 저하**: 모든 기능이 한 파일에 집중되어 파일 관리가 어렵습니다.
- **유지보수 어려움**: 라우트와 핸들러를 분리하지 않아 수정 및 확장이 번거롭습니다.

아래는 기존의 `routes/campgrounds.js` 코드입니다.

```javascript
// routes/campgrounds.js
const express = require('express');
const router = express.Router();
const catchAsync = require('../utils/catchAsync');
const { isLoggedIn, isAuthor, validateCampground } = require('../middleware');
const Campground = require('../models/campground');

router.get('/', catchAsync(async (req, res) => {
    const campgrounds = await Campground.find({});
    res.render('campgrounds/index', { campgrounds });
}));

router.get('/new', isLoggedIn, (req, res) => {
    res.render('campgrounds/new');
});

router.post('/', isLoggedIn, validateCampground, catchAsync(async (req, res) => {
    const campground = new Campground(req.body.campground);
    campground.author = req.user._id;
    await campground.save();
    req.flash('success', 'Successfully made a new campground!');
    res.redirect(`/campgrounds/${campground._id}`);
}));

router.get('/:id', catchAsync(async (req, res) => {
    const campground = await Campground.findById(req.params.id).populate({
        path: 'reviews',
        populate: { path: 'author' }
    }).populate('author');
    if (!campground) {
        req.flash('error', 'Cannot find that campground!');
        return res.redirect('/campgrounds');
    }
    res.render('campgrounds/show', { campground });
}));

router.get('/:id/edit', isLoggedIn, isAuthor, catchAsync(async (req, res) => {
    const { id } = req.params;
    const campground = await Campground.findById(id);
    if (!campground) {
        req.flash('error', 'Cannot find that campground!');
        return res.redirect('/campgrounds');
    }
    res.render('campgrounds/edit', { campground });
}));

router.put('/:id', isLoggedIn, isAuthor, validateCampground, catchAsync(async (req, res) => {
    const { id } = req.params;
    await Campground.findByIdAndUpdate(id, { ...req.body.campground });
    req.flash('success', 'Successfully updated campground!');
    res.redirect(`/campgrounds/${id}`);
}));

router.delete('/:id', isLoggedIn, isAuthor, catchAsync(async (req, res) => {
    const { id } = req.params;
    await Campground.findByIdAndDelete(id);
    req.flash('success', 'Successfully deleted campground!');
    res.redirect('/campgrounds');
}));

module.exports = router;
```

## 리팩토링 목표

1. **컨트롤러 분리**: 라우트 핸들러를 별도 파일로 이동하여 모듈화합니다.
2. **`router.route` 활용**: RESTful 라우트를 그룹화하여 중복을 줄입니다.
3. **미들웨어 정리**: 미들웨어를 명확히 정의하고 필요한 라우트에만 적용합니다.

---

## 리팩토링 과정

### 1. 컨트롤러 분리

라우트 핸들러를 `controllers/campgrounds.js` 파일로 분리합니다. 이렇게 하면 라우트 정의는 간결해지고, 핸들러 함수는 독립적으로 관리할 수 있습니다.

```javascript
// controllers/campgrounds.js
const Campground = require('../models/campground');

module.exports.index = async (req, res) => {
    const campgrounds = await Campground.find({});
    res.render('campgrounds/index', { campgrounds });
};

module.exports.renderNewForm = (req, res) => {
    res.render('campgrounds/new');
};

module.exports.createCampground = async (req, res) => {
    const campground = new Campground(req.body.campground);
    campground.author = req.user._id;
    await campground.save();
    req.flash('success', 'Successfully made a new campground!');
    res.redirect(`/campgrounds/${campground._id}`);
};

module.exports.showCampground = async (req, res) => {
    const campground = await Campground.findById(req.params.id).populate({
        path: 'reviews',
        populate: { path: 'author' }
    }).populate('author');
    if (!campground) {
        req.flash('error', 'Cannot find that campground!');
        return res.redirect('/campgrounds');
    }
    res.render('campgrounds/show', { campground });
};

module.exports.renderEditForm = async (req, res) => {
    const campground = await Campground.findById(req.params.id);
    if (!campground) {
        req.flash('error', 'Cannot find that campground!');
        return res.redirect('/campgrounds');
    }
    res.render('campgrounds/edit', { campground });
};

module.exports.updateCampground = async (req, res) => {
    const { id } = req.params;
    await Campground.findByIdAndUpdate(id, { ...req.body.campground });
    req.flash('success', 'Successfully updated campground!');
    res.redirect(`/campgrounds/${id}`);
};

module.exports.deleteCampground = async (req, res) => {
    await Campground.findByIdAndDelete(req.params.id);
    req.flash('success', 'Successfully deleted campground!');
    res.redirect('/campgrounds');
};
```

### 2. `router.route` 활용

`router.route`를 사용하여 동일한 경로의 라우트를 그룹화합니다. 이렇게 하면 코드 중복이 줄고, 가독성이 향상됩니다.

```javascript
// routes/campgrounds.js
const express = require('express');
const router = express.Router();
const campgrounds = require('../controllers/campgrounds');
const { isLoggedIn, validateCampground, isAuthor } = require('../middleware');

router.route('/')
    .get(campgrounds.index)
    .post(isLoggedIn, validateCampground, campgrounds.createCampground);

router.route('/new')
    .get(isLoggedIn, campgrounds.renderNewForm);

router.route('/:id')
    .get(campgrounds.showCampground)
    .put(isLoggedIn, isAuthor, validateCampground, campgrounds.updateCampground)
    .delete(isLoggedIn, isAuthor, campgrounds.deleteCampground);

router.route('/:id/edit')
    .get(isLoggedIn, isAuthor, campgrounds.renderEditForm);

module.exports = router;
```

---

## 리팩토링 결과

### 리팩토링 전:
- 모든 핸들러가 한 파일에 몰려 있음.
- 코드 중복으로 인해 유지보수가 어려움.

### 리팩토링 후:
- **컨트롤러 분리**: 라우트와 핸들러를 분리해 코드 가독성 및 모듈화 향상.
- **라우트 병합**: `router.route` 활용으로 코드 중복 제거.
- **미들웨어 정리**: 필요한 곳에만 미들웨어를 적용하여 가독성 개선.

---

Express 애플리케이션의 라우트를 개선하고 싶다면 위와 같은 접근법을 활용해 보세요. 프로젝트 확장성과 유지보수성이 크게 향상될 것입니다! 😊

