---
title: "Node.js 미들웨어로 로그인 상태 확인하기"
excerpt: "로그인 여부를 확인하는 미들웨어를 구현하고, 특정 경로에 인증된 사용자만 접근 가능하도록 설정하는 방법을 알아봅니다."
categories:
  - Node
  - Express
  - Web Development
tags:
  - [JavaScript, Node.js, Express, Middleware, Authentication]
permalink: /node/middleware-authentication/
toc: true
toc_sticky: true
date: 2025-01-06
last_modified_at: 2025-01-06
---

# 로그인한 상태에서만 접근 가능하도록 구현한 미들웨어 설명

이 글에서는 사용자가 로그인한 상태에서만 특정 페이지에 접근할 수 있도록 설정하는 방법을 다룹니다. `Express.js`를 기반으로 한 미들웨어를 작성하여, 사용자가 인증되지 않은 상태라면 로그인 페이지로 리다이렉트되도록 구현했습니다.

## **1. 미들웨어: `isLoggedIn`**
`middleware.js` 파일에 정의된 `isLoggedIn` 미들웨어는 다음과 같은 역할을 수행합니다.

```javascript
module.exports.isLoggedIn = (req, res, next) => {
    if (!req.isAuthenticated()) { // 사용자가 인증되지 않은 경우
        req.flash('error', 'You must be signed in first!'); // 에러 메시지 추가
        req.session.returnTo = req.originalUrl; // 사용자가 요청한 URL 저장
        return res.redirect('/login'); // 로그인 페이지로 리다이렉트
    }
    next(); // 인증된 사용자인 경우, 다음 미들웨어 또는 라우트 핸들러로 이동
};
```

### **주요 기능**
1. **`req.isAuthenticated()` 확인**:
   - Passport.js에서 제공하는 메서드로, 사용자가 로그인 상태인지 확인합니다.
   - 인증되지 않은 경우 조건문이 실행됩니다.
   
2. **`req.flash` 사용**:
   - 사용자가 로그인하지 않았다는 오류 메시지를 생성합니다.
   - 이 메시지는 플래시 메시징 라이브러리(예: `connect-flash`)를 사용하여 로그인 페이지에서 사용자에게 표시됩니다.

3. **`req.session.returnTo`**:
   - 사용자가 요청한 URL을 세션에 저장합니다.
   - 로그인 후 원래 요청했던 페이지로 리다이렉트하기 위해 사용됩니다.

4. **리다이렉트 처리**:
   - 인증되지 않은 사용자는 `/login` 페이지로 리다이렉트됩니다.
   - 로그인된 사용자는 `next()`를 호출하여 다음 단계로 진행합니다.

---

## **2. 라우트에서 미들웨어 사용**
`campgrounds.js`에서 특정 경로에 `isLoggedIn` 미들웨어를 적용합니다.

```javascript
router.get('/new', isLoggedIn, (req, res) => {
    res.render('campgrounds/new'); // 인증된 사용자가 새 캠프장 작성 페이지를 볼 수 있음
});
```

### **동작 설명**
1. 사용자가 `/new` 경로로 요청을 보냅니다.
2. `isLoggedIn` 미들웨어가 실행됩니다.
   - 사용자가 로그인한 상태라면 `next()`를 호출해 `res.render('campgrounds/new')`로 이동합니다.
   - 로그인하지 않은 상태라면 `/login` 페이지로 리다이렉트됩니다.

---

## **3. 플로우 요약**
1. **로그인되지 않은 경우**:
   - 사용자가 `/new` 경로에 접근 → `isLoggedIn` 실행 → 세션에 요청한 URL 저장 → `/login`으로 리다이렉트 → 로그인 완료 시 저장된 URL(`/new`)로 리다이렉트.

2. **로그인된 경우**:
   - 사용자가 `/new` 경로에 접근 → `isLoggedIn` 실행 → `next()` 호출 → 캠프장 작성 페이지 렌더링.

---

## **4. 응용**
이 미들웨어는 다른 라우트에도 재사용할 수 있습니다. 예를 들어, `/edit` 또는 `/delete` 같은 경로에서 인증 상태를 확인하려면 동일한 미들웨어를 추가하면 됩니다.

```javascript
router.get('/edit/:id', isLoggedIn, (req, res) => {
    // 캠프장 수정 페이지 렌더링
});
```

---

## **5. 결론**
`isLoggedIn` 미들웨어는 간단하면서도 효율적으로 인증 상태를 확인합니다. 이를 통해 인증되지 않은 사용자가 특정 페이지에 접근하지 못하도록 보호할 수 있습니다. 추가적으로 `req.session.returnTo`를 활용해 사용자 경험을 개선할 수 있습니다.

이 구현은 인증 관련 로직을 분리하여 코드의 가독성을 높이고 유지보수를 쉽게 만듭니다.

