---
title: "Joi와 sanitize-html을 사용한 안전한 유효성 검사 구현"
excerpt: "Joi 확장(extension)과 sanitize-html 라이브러리를 활용하여 입력 데이터의 보안을 강화하고 XSS 공격을 방지하는 방법을 소개합니다."
categories:
  - Node
  - JavaScript
  - Web Security
tags:
  - [Node.js, Joi, sanitize-html, Validation, XSS Prevention]
permalink: /node/joi-sanitize-html/
toc: true
toc_sticky: true
date: 2025-01-10
last_modified_at: 2025-01-10
---

## Joi와 sanitize-html을 활용한 안전한 유효성 검사

웹 애플리케이션에서 사용자 입력은 가장 일반적인 보안 취약점의 원천입니다. 특히, 크로스 사이트 스크립팅(XSS) 공격은 사용자 입력 데이터에 포함된 악성 HTML 및 JavaScript 코드를 실행하게 할 수 있습니다. 이번 포스팅에서는 **Joi**와 **sanitize-html** 라이브러리를 활용하여 안전한 유효성 검사를 구현하는 방법을 알아보겠습니다.

### 기본 Joi 스키마
기존의 Joi 스키마는 입력 데이터를 유효성 검사하는 데 매우 유용하지만, HTML 태그와 같은 잠재적 보안 위협 요소를 자동으로 제거하지는 않습니다. 아래는 기본 Joi 스키마 예시입니다.

```javascript
const Joi = require('joi');

module.exports.campgroundSchema = Joi.object({
    campground: Joi.object({
        title: Joi.string().required(),
        price: Joi.number().required().min(0),
        location: Joi.string().required(),
        description: Joi.string().required()
    }).required(),
    deleteImages: Joi.array()
});

module.exports.reviewSchema = Joi.object({
    review: Joi.object({
        rating: Joi.number().required().max(5).min(0),
        body: Joi.string().required(),
    }).required()
});

module.exports.userSchema = Joi.object({
    user: Joi.object({
        username: Joi.string().required(),
        password: Joi.string().required(),
        email: Joi.string().required(),
    }).required()
});
```

### sanitize-html과 Joi 확장 사용하기
**sanitize-html**을 사용하여 HTML 태그를 제거하고 이를 Joi 스키마에 통합하려면, 확장을 만들어야 합니다. 아래는 `escapeHTML`이라는 커스텀 Joi 확장을 추가한 코드입니다.

```javascript
const BaseJoi = require('joi');
const sanitizeHtml = require('sanitize-html');

const extension = (joi) => ({
    type: 'string',
    base: joi.string(),
    messages: {
        'string.escapeHTML': '{{#label}} must not include HTML!'
    },
    rules: {
        escapeHTML: {
            validate(value, helpers) {
                const clean = sanitizeHtml(value, {
                    allowedTags: [],
                    allowedAttributes: {},
                });
                if (clean !== value)
                    return helpers.error('string.escapeHTML', { value });
                return clean;
            }
        }
    }
});

const Joi = BaseJoi.extend(extension);

module.exports.campgroundSchema = Joi.object({
    campground: Joi.object({
        title: Joi.string().required().escapeHTML(),
        price: Joi.number().required().min(0),
        location: Joi.string().required().escapeHTML(),
        description: Joi.string().required().escapeHTML()
    }).required(),
    deleteImages: Joi.array()
});

module.exports.reviewSchema = Joi.object({
    review: Joi.object({
        rating: Joi.number().required().min(1).max(5),
        body: Joi.string().required().escapeHTML()
    }).required()
});

module.exports.userSchema = Joi.object({
    user: Joi.object({
        username: Joi.string().required().escapeHTML(),
        password: Joi.string().required().escapeHTML(),
        email: Joi.string().required().escapeHTML(),
    }).required()
});
```

### 주요 변경 사항 설명

1. **Custom Joi 확장(extension) 정의**:
   - `sanitize-html` 라이브러리를 사용하여 HTML 태그를 제거하는 `escapeHTML` 규칙을 정의했습니다.
   - `type: 'string'`으로 문자열 값에만 적용되며, 태그가 포함된 경우 에러 메시지를 반환합니다.

2. **HTML 제거 로직**:
   - `sanitizeHtml` 함수로 사용자 입력 데이터에서 허용되지 않은 태그와 속성을 제거합니다.
   - 원래 값과 정리된 값을 비교하여, 값이 다르면 에러를 반환합니다.

3. **스키마 업데이트**:
   - 모든 문자열 필드(`title`, `location`, `description`, `body`, `username`, `password`, `email`)에 `escapeHTML()` 규칙을 추가했습니다.

### 코드의 장점

1. **보안 강화**:
   - HTML 태그와 속성을 제거하여 XSS 공격을 효과적으로 방지할 수 있습니다.

2. **재사용 가능성**:
   - 한 번 정의한 `escapeHTML` 규칙을 필요할 때마다 간단히 문자열 필드에 추가할 수 있습니다.

3. **가독성 및 유지보수성**:
   - Joi 스키마에 직접 통합되어 코드가 간결하고 명확합니다.

### 결론
위의 방식으로 `Joi`와 `sanitize-html`을 결합하여 안전한 유효성 검사를 구현할 수 있습니다. 이러한 설정은 사용자 입력 데이터를 처리할 때 발생할 수 있는 보안 문제를 예방하고, 더욱 안전한 웹 애플리케이션을 개발하는 데 기여할 것입니다.

