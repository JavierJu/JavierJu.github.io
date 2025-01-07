---
title: "Node.js 미들웨어로 게시물 권한 관리하기"
excerpt: "Node.js와 Express를 활용하여 사용자가 작성한 게시물만 수정할 수 있도록 권한을 관리하는 방법을 알아봅니다."
categories:
  - Node
  - Express
  - Web Development
tags:
  - [Node.js, Express, Middleware, Authorization, Web Development]
permalink: /node/authorization-middleware/
toc: true
toc_sticky: true
date: 2025-01-06
last_modified_at: 2025-01-06
---

## 사용자가 작성한 게시물만 수정 가능하도록 권한 관리하기

Node.js와 Express를 사용한 웹 애플리케이션에서는 사용자가 자신이 작성하지 않은 게시물을 수정하거나 삭제하지 못하도록 하는 권한 관리가 중요합니다. 이를 위해 미들웨어를 작성하고 적용하는 과정을 살펴보겠습니다.

### 미들웨어 구현 (`middleware.js`)

다음은 사용자가 요청한 `campground`의 작성자인지 확인하는 미들웨어 코드입니다:

```javascript
module.exports.isAuthor = async (req, res, next) => {
    const campground = await Campground.findById(req.params.id);
    if (!campground.author.equals(req.user._id)) {
        req.flash('error', 'You do not have a permission!');
        return res.redirect(`/campgrounds/${campground._id}`);
    }
    next();
};
```

#### 주요 기능
1. **요청된 `campground` 데이터 가져오기**:
   - `Campground.findById(req.params.id)`로 현재 요청된 게시물을 데이터베이스에서 찾습니다.
2. **작성자 확인**:
   - `campground.author.equals(req.user._id)`를 통해 게시물 작성자와 현재 로그인한 사용자의 ID를 비교합니다.
3. **권한 없을 시 처리**:
   - 작성자가 아닐 경우 플래시 메시지를 설정하고, 해당 게시물 페이지로 리다이렉트합니다.
4. **권한 있을 시 다음 단계로 이동**:
   - 작성자가 맞다면 `next()`를 호출하여 다음 미들웨어 또는 요청 처리를 진행합니다.

---

### 라우트에서 미들웨어 사용 (`campgrounds.js`)

이 미들웨어를 특정 라우트에 적용하여 게시물 수정 페이지에 접근할 수 있는 사용자를 제한합니다:

```javascript
router.get('/:id/edit', isLoggedIn, isAuthor, async (req, res) => {
    const campground = await Campground.findById(req.params.id);
    if (!campground) {
        req.flash('error', 'Cannot find that campground!');
        return res.redirect('/campgrounds');
    }
    res.render('campgrounds/edit', { campground });
});
```

#### 코드 설명
1. **미들웨어 체인 구성**:
   - `isLoggedIn`: 사용자가 로그인했는지 확인.
   - `isAuthor`: 사용자가 게시물의 작성자인지 확인.
2. **게시물 존재 여부 확인**:
   - `campground`가 없을 경우 에러 메시지를 표시하고 목록 페이지로 리다이렉트합니다.
3. **수정 페이지 렌더링**:
   - 모든 조건을 통과하면 수정 페이지를 렌더링합니다.

---

### 클라이언트 측 UI 조건부 렌더링 (`show.ejs`)

작성자인 경우에만 수정 및 삭제 버튼이 표시되도록 템플릿에 조건을 추가합니다:

```html
<% if (currentUser && campground.author.equals(currentUser._id)) { %>
    <div class="card-body">
        <a href="/campgrounds/<%= campground._id %>/edit" class="card-link btn btn-warning">Edit</a>
        <form action="/campgrounds/<%= campground._id %>?_method=Delete" method="post" class="d-inline">
            <button type="submit" class="card-link btn btn-danger">Delete</button>
        </form>
    </div>
<% } %>
```

#### 동작 방식
1. **조건 확인**:
   - `currentUser`가 로그인 상태인지 확인.
   - `campground.author.equals(currentUser._id)`로 현재 사용자가 작성자인지 확인.
2. **UI 제어**:
   - 조건을 충족할 때만 "Edit" 및 "Delete" 버튼이 렌더링됩니다.

---

### 전체 동작 과정 요약
1. **사용자가 수정 페이지에 접근**:
   - `/campgrounds/:id/edit`로 GET 요청.
2. **서버 측 확인**:
   - `isLoggedIn`으로 로그인 여부 확인.
   - `isAuthor`로 작성자 여부 확인.
3. **권한 부여 또는 차단**:
   - 작성자가 아닐 경우 에러 메시지와 함께 리다이렉트.
   - 작성자인 경우 수정 페이지 렌더링.
4. **클라이언트 측 보호**:
   - 작성자가 아닌 사용자는 UI에서 수정/삭제 버튼 비활성화.

---

### 보안 및 UX 개선 포인트
- **서버 측 보호**:
  - 미들웨어로 권한을 철저히 검증하여 불법 접근을 방지.
- **클라이언트 측 보호**:
  - UI에서 버튼 노출을 제한하여 불필요한 액션을 줄임.
- **에러 처리**:
  - `req.flash`로 사용자에게 적절한 피드백 제공.

