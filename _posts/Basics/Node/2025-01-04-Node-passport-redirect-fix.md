---
title: "Passport.js에서 로그인 후 원래 페이지로 리디렉션 문제 해결"
excerpt: "Passport.js의 최신 업데이트로 인해 로그인 후 세션이 재생성되면서 원래 페이지로 리디렉션되지 않는 문제를 해결하는 방법을 알아봅니다."
categories:
  - Node
  - Authentication
  - Express
tags:
  - [JavaScript, Node.js, Passport.js, Authentication, Express]
permalink: /node/passport-redirect-fix/
toc: true
toc_sticky: true
date: 2025-01-04
last_modified_at: 2025-01-04
---

Passport.js를 사용하여 인증을 구현하는 애플리케이션에서 최근 업데이트로 인해 로그인 후 원래 방문하려던 페이지로 리디렉션되지 않는 문제가 발생할 수 있습니다. 이는 세션이 로그인 시 재생성되면서 발생하며, 이를 해결하기 위한 방법을 소개합니다.

## 문제의 원인

Passport.js는 보안 강화를 위해 사용자가 로그인하면 기존 세션을 삭제하고 새 세션을 생성합니다. 이 과정에서 로그인 전에 세션에 저장했던 데이터(`req.session.returnTo`)가 손실될 수 있습니다. 따라서, 로그인 후 원래 요청한 페이지로 리디렉션하려는 로직이 제대로 동작하지 않습니다.

## 해결 방법

### 1. 세션 재생성 전에 데이터 임시 저장

로그인 처리 로직에서 세션 재생성 전에 `req.session.returnTo` 값을 임시로 저장한 후, 새로운 세션에 복원하는 방법입니다.

```javascript
app.post('/login', (req, res, next) => {
    const returnTo = req.session.returnTo; // returnTo 값을 임시로 저장
    passport.authenticate('local', (err, user, info) => {
        if (err) return next(err);
        if (!user) {
            req.flash('error', info.message);
            return res.redirect('/login');
        }
        req.logIn(user, (err) => {
            if (err) return next(err);
            req.session.returnTo = returnTo; // 로그인 후 세션에 복원
            const redirectUrl = returnTo || '/';
            res.redirect(redirectUrl);
        });
    })(req, res, next);
});
```

#### 코드 설명
- `req.session.returnTo`: 사용자가 로그인 전에 요청한 URL을 임시로 변수에 저장합니다.
- `req.logIn`: Passport.js의 메서드로, 로그인 후 새로운 세션을 생성합니다.
- 새로운 세션이 생성된 후, 임시로 저장한 `returnTo` 값을 세션에 다시 복원합니다.

### 2. `connect-flash`를 활용한 대안

세션이 재생성되더라도 `connect-flash`를 사용하여 `returnTo` 값을 유지할 수 있습니다.

```javascript
// isLogedIn 미들웨어
module.exports.isLogedIn = (req, res, next) => {
    if (!req.isAuthenticated()) {
        req.flash('returnTo', req.originalUrl); // returnTo 값을 flash에 저장
        req.flash('error', 'You must be signed in first!');
        return res.redirect('/login');
    }
    next();
};

// 로그인 처리
app.post('/login', passport.authenticate('local', {
    failureRedirect: '/login',
    failureFlash: true
}), (req, res) => {
    const redirectUrl = req.flash('returnTo')[0] || '/'; // flash에서 returnTo 가져오기
    res.redirect(redirectUrl);
});
```

#### 코드 설명
- `req.flash('returnTo')`: `returnTo` 값을 세션 대신 플래시 메시지로 저장합니다.
- 로그인 성공 후 플래시 메시지에서 `returnTo` 값을 읽어와 사용자를 원래 페이지로 리디렉션합니다.

### 3. `res.locals`를 활용한 미들웨어

`storeReturnTo` 미들웨어를 사용하여 세션의 `returnTo` 값을 `res.locals`에 저장하고, 로그인 후 해당 값을 사용하는 방법입니다.

```javascript
module.exports.storeReturnTo = (req, res, next) => {
    if (req.session.returnTo) {
        res.locals.returnTo = req.session.returnTo;
    }
    next();
};

// 라우트 설정
const { storeReturnTo } = require('../middleware');
router.post('/login', storeReturnTo, passport.authenticate('local', {
    failureFlash: true,
    failureRedirect: '/login'
}), async (req, res) => {
    req.flash('success', 'welcome back');
    const redirectUrl = res.locals.returnTo || '/campgrounds';
    res.redirect(redirectUrl);
});
```

#### 코드 설명
- `storeReturnTo` 미들웨어는 `req.session.returnTo` 값을 `res.locals.returnTo`에 저장합니다.
- 로그인 후 `res.locals.returnTo` 값을 사용하여 리디렉션합니다.

## 전체 동작 흐름

1. 사용자가 보호된 페이지에 접근하면 `isLogedIn` 미들웨어가 호출됩니다.
2. 로그인되지 않은 경우, 원래 요청 URL을 세션 또는 플래시에 저장한 후 로그인 페이지로 리디렉션합니다.
3. 로그인 후, 저장된 URL로 사용자를 리디렉션합니다.

## 결론

Passport.js의 세션 재생성으로 인한 원래 페이지 리디렉션 문제는 **임시 저장과 복원**, **`connect-flash` 활용**, 또는 **`res.locals`를 활용한 미들웨어**로 해결할 수 있습니다. 각 방법은 구현이 간단하며, 애플리케이션의 인증 흐름을 개선하는 데 유용합니다.

