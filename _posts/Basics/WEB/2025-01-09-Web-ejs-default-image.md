---
title: "EJS에서 이미지가 없을 때 기본 이미지를 설정하는 방법"
excerpt: "EJS 템플릿에서 이미지가 없는 경우 오류를 방지하고 기본 이미지를 표시하는 방법을 단계별로 알아봅니다."
categories:
  - Web
tags:
  - [EJS, Node.js, Express, Web Development]
permalink: /web/ejs-default-image/
toc: true
toc_sticky: true
date: 2025-01-09
last_modified_at: 2025-01-09
---

EJS 템플릿에서 `campground.images[0]`와 같은 배열 요소를 참조할 때 해당 배열이 비어 있거나 정의되지 않은 경우, 오류가 발생할 수 있습니다. 이 글에서는 이미지가 없는 경우 기본 이미지를 표시하도록 설정하는 방법을 살펴보겠습니다.

## 문제 상황
아래와 같은 EJS 템플릿 코드가 있다고 가정합니다:

```ejs
<h1>All Campgrounds</h1>
<% for( let campground of campgrounds ) { %>
    <div class="card mb-3">
        <div class="row">
            <div class="col-md-4">
                <img src="<%= campground.images[0].url %>" class="img-fluid" alt="">
            </div>
        </div>
    </div>
<% } %>
```

위 코드는 `campground.images[0]`가 정의되지 않은 경우 다음과 같은 오류를 발생시킵니다:

```
Cannot read properties of undefined (reading 'url')
```

## 해결 방법
이미지가 없는 경우 기본 이미지를 표시하도록 코드를 수정합니다. 이를 위해 조건부 논리를 추가할 수 있습니다.

### 수정된 코드

```ejs
<h1>All Campgrounds</h1>
<% for (let campground of campgrounds) { %>
    <div class="card mb-3">
        <div class="row">
            <div class="col-md-4">
                <img
                    src="<%= (campground.images && campground.images.length > 0) ? campground.images[0].url : '/images/default-image.jpg' %>"
                    class="img-fluid"
                    alt="Campground image">
            </div>
        </div>
    </div>
<% } %>
```

### 코드 설명
1. **조건문 추가**:
   - `campground.images && campground.images.length > 0`: `campground.images`가 정의되어 있고 배열이 비어 있지 않은지 확인합니다.
   - 조건이 참이면 `campground.images[0].url`을 사용하고, 그렇지 않으면 기본 이미지 경로를 반환합니다.

2. **기본 이미지 경로 설정**:
   - 기본 이미지는 프로젝트의 `public` 디렉토리에 저장합니다.
   - 예: `public/images/default-image.jpg`

3. **보안 고려**:
   - 이미지 URL이 외부에서 제공되는 경우, 신뢰할 수 있는 소스인지 검증하는 것이 중요합니다.

## 기본 이미지 설정하기

### 기본 이미지 파일 추가

1. `public/images/default-image.jpg` 경로에 기본 이미지를 저장합니다.
2. Express는 `public` 디렉토리를 정적 파일의 기본 디렉토리로 사용하므로, 경로 설정이 간단합니다.

### Express 설정 확인

Express 앱이 정적 파일을 서빙하도록 설정되었는지 확인합니다:

```javascript
const express = require('express');
const app = express();

// 정적 파일 디렉토리 설정
app.use(express.static('public'));
```

## 결과
이제 `campground.images` 배열이 비어 있거나 정의되지 않았을 때, 기본 이미지가 표시됩니다. 오류 없이 사용자에게 더 나은 경험을 제공할 수 있습니다.

