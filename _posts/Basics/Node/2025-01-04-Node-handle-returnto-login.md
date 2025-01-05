---
title: "Node.js 로그인 라우터에서 안전한 리다이렉트 처리 방법"
excerpt: "로그인 후 리다이렉트 URL을 적절히 관리하여 API 엔드포인트로의 잘못된 리다이렉트를 방지하는 방법을 다룹니다."
categories:
  - Node
  - Express
  - Web Development
tags:
  - [Node.js, Express, Middleware, Authentication, Redirect]
permalink: /node/handle-returnto-login/
toc: true
toc_sticky: true
date: 2025-01-04
last_modified_at: 2025-01-04
---

Node.js와 Express를 사용해 로그인 기능을 구현할 때, 인증이 필요한 페이지로 접근하면 로그인 후 해당 페이지로 리다이렉트되도록 설계하는 것이 일반적입니다. 하지만 이 과정에서 API 엔드포인트로 리다이렉트되는 문제가 발생할 수 있습니다. 본 포스트에서는 이를 방지하는 안전한 방법을 소개합니다.

## 문제 상황

아래는 일반적인 로그인 라우터 코드입니다:

```javascript
router.post('/login', storeReturnTo, passport.authenticate('local', { failureFlash: true, failureRedirect: '/login' }), async (req, res) => {
    req.flash('success', 'Welcome back!');
    const redirectUrl = res.locals.returnTo || '/campgrounds';
    res.redirect(redirectUrl);
});
```

이 코드에서는 `res.locals.returnTo` 값을 사용해 로그인 후 사용자가 원래 요청했던 URL로 리다이렉트합니다. 하지만 문제가 되는 경우는 다음과 같습니다:

- 사용자가 `/campgrounds/:id/reviews` API 엔드포인트로 접근했을 때, 로그인 후에도 이 API 엔드포인트로 리다이렉트됩니다.
- `/campgrounds/:id/reviews`는 실제로 별도의 페이지가 없으므로 사용자는 적절한 부모 페이지(`/campgrounds/:id`)로 이동해야 합니다.

## 해결 방법: URL 검증 및 수정

로그인 라우터에서 `res.locals.returnTo` 값을 확인하고, 필요한 경우 적절한 부모 페이지로 수정합니다. 아래는 수정된 코드입니다:

```javascript
router.post('/login', storeReturnTo, passport.authenticate('local', { failureFlash: true, failureRedirect: '/login' }), async (req, res) => {
    req.flash('success', 'Welcome back!');

    // res.locals.returnTo에 저장된 URL 검증 및 수정
    let redirectUrl = res.locals.returnTo || '/campgrounds';

    // '/campgrounds/:id/reviews'인 경우 부모 페이지로 수정
    const match = redirectUrl.match(/^\/campgrounds\/([^/]+)\/reviews/);
    if (match) {
        redirectUrl = `/campgrounds/${match[1]}`;
    }

    res.redirect(redirectUrl); // 수정된 URL로 리다이렉트
});
```

### 코드 설명

1. **`res.locals.returnTo` 확인**:
   - `res.locals.returnTo` 값이 `/campgrounds/:id/reviews`와 같은 API 엔드포인트인지 정규식을 사용해 검사합니다.

2. **정규식 매칭**:
   - 정규식 `^\/campgrounds\/([^/]+)\/reviews`를 사용하여 URL이 `/campgrounds/:id/reviews` 형태인지 확인하고, `:id` 값을 추출합니다.

3. **URL 수정**:
   - API 엔드포인트(`/campgrounds/:id/reviews`)를 부모 페이지(`/campgrounds/:id`)로 수정합니다.

4. **리다이렉트**:
   - 수정된 `redirectUrl`로 사용자를 리다이렉트합니다.

## 테스트 시나리오

1. **리뷰 작성 요청**:
   - 사용자가 `/campgrounds/:id/reviews`로 접근한 상태에서 로그인하면, `/campgrounds/:id`로 리다이렉트됩니다.

2. **다른 페이지 요청**:
   - `/campgrounds/:id`와 같은 실제 페이지는 그대로 리다이렉트됩니다.

3. **기본 동작 확인**:
   - 로그인 후 리다이렉트 URL이 설정되지 않은 경우, `/campgrounds`로 이동합니다.

## 결론

이번 포스트에서는 Node.js와 Express로 구현된 로그인 라우터에서 잘못된 리다이렉트를 방지하는 방법을 살펴보았습니다. 이 방법은 사용자 경험을 개선하고, API 엔드포인트로 리다이렉트되는 문제를 효과적으로 해결할 수 있습니다.

### 관련 링크
- [Express 공식 문서](https://expressjs.com/)
- [Passport.js 문서](http://www.passportjs.org/)

