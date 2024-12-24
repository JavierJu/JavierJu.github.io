---
title: "Joi를 이용한 유효성 검사 코드 리팩토링 및 개선"
excerpt: "Joi를 활용한 유효성 검사 코드를 개선하고 유지보수성을 높이는 리팩토링 방법을 알아봅니다."
categories:
  - Node
tags:
  - [Joi, Validation, Node.js, Backend]
permalink: /Node/joi-validation-refactor/
toc: true
toc_sticky: true
date: 2024-12-24
last_modified_at: 2024-12-24
---

`Joi`는 JavaScript에서 객체의 유효성 검사를 간단하고 효과적으로 수행할 수 있는 라이브러리입니다. 아래에서는 기존 `Joi` 유효성 검사 코드를 분석하고 이를 리팩토링하여 가독성과 유지보수성을 높이는 방법을 알아보겠습니다.

---

## 기존 코드 분석
### `schemas.js`
```javascript
const Joi = require('joi');

module.exports.campgroundSchema = Joi.object({
    campground: Joi.object({
        title: Joi.string().required(),
        price: Joi.number().required().min(0),
        image: Joi.string().required(),
        location: Joi.string().required(),
        description: Joi.string().required()
    }).required()
});
```

### `app.js`
```javascript
const { campgroundSchema } = require('./schemas');

const validateCampground = (req, res, next) => {
    const { error } = campgroundSchema.validate(req.body);
    if (error) {
        const msg = error.details.map(el => el.message).join(',');
        throw new ExpressError(msg, 400);
    } else {
        next();
    }
};

app.post('/campgrounds', validateCampground, async (req, res) => {
    const newCampground = new Campground(req.body.campground);
    await newCampground.save();
    res.redirect(`/campgrounds/${newCampground._id}`);
});
```

위 코드의 주요 기능은 클라이언트 요청의 데이터를 `Joi`를 사용해 검증한 후, 문제가 없을 경우 다음 단계로 넘어가게 하는 것입니다.

---

## 리팩토링 및 개선 사항

### 1. 에러 메시지 커스터마이징
현재 에러 메시지는 `Joi`에서 기본 제공하는 메시지를 사용하고 있습니다. 이를 사용자 친화적으로 커스터마이징할 수 있습니다.

#### 리팩토링된 코드 (schemas.js)
```javascript
const Joi = require('joi');

const campgroundBase = {
    title: Joi.string().required().messages({
        'string.empty': 'Title is required.',
        'any.required': 'Title is required.',
    }),
    price: Joi.number().required().min(0).messages({
        'number.base': 'Price must be a number.',
        'number.min': 'Price must be at least 0.',
        'any.required': 'Price is required.',
    }),
    image: Joi.string().required().messages({
        'string.empty': 'Image URL is required.',
        'any.required': 'Image URL is required.',
    }),
    location: Joi.string().required().messages({
        'string.empty': 'Location is required.',
        'any.required': 'Location is required.',
    }),
    description: Joi.string().required().messages({
        'string.empty': 'Description is required.',
        'any.required': 'Description is required.',
    }),
};

module.exports.campgroundSchema = Joi.object({
    campground: Joi.object(campgroundBase).required(),
});
```

### 2. 유효성 검증 미들웨어 모듈화
유효성 검증 로직을 별도의 파일로 분리하여 코드 관리와 테스트를 쉽게 만듭니다.

#### 리팩토링된 코드 (middleware/validateCampground.js)
```javascript
const { campgroundSchema } = require('../schemas');
const ExpressError = require('../utils/ExpressError');

const validateCampground = (req, res, next) => {
    const { error } = campgroundSchema.validate(req.body, { abortEarly: false });
    if (error) {
        const msg = error.details.map(el => el.message).join(', ');
        throw new ExpressError(msg, 400);
    }
    next();
};

module.exports = validateCampground;
```

### 3. 가독성 및 유지보수성 향상
에러 메시지 처리 로직과 스키마 정의를 개선하여 가독성과 유지보수성을 높였습니다.

#### 통합된 코드 (app.js)
```javascript
const validateCampground = require('./middleware/validateCampground');

app.post('/campgrounds', validateCampground, async (req, res) => {
    const newCampground = new Campground(req.body.campground);
    await newCampground.save();
    res.redirect(`/campgrounds/${newCampground._id}`);
});
```

---

## 주요 변경점 요약
1. **에러 메시지 커스터마이징**: 필드별로 구체적이고 사용자 친화적인 메시지를 제공합니다.
2. **유효성 검사 로직 분리**: 미들웨어를 별도 파일로 분리하여 코드 구조를 간결하게 만듭니다.
3. **확장성 및 유지보수성 향상**: `abortEarly: false` 옵션을 통해 모든 에러를 한 번에 검출하고, 코드 재사용성을 높였습니다.

---

Joi를 활용한 유효성 검증 코드를 이와 같이 리팩토링하면 코드의 가독성과 확장성이 크게 향상됩니다. 특히 대규모 프로젝트에서 이러한 모듈화와 개선 작업은 코드의 유지보수성과 협업 효율성을 높이는 데 크게 기여할 것입니다.

