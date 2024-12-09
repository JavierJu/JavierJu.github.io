---
title: "JavaScript에서 .params 메서드의 이해: Express.js에서 라우트 매개변수 처리"
excerpt: "JavaScript에서 .params는 Express.js의 라우트 매개변수를 처리하는 중요한 속성입니다. 이 글에서는 .params의 사용법과 활용 예시를 설명합니다."
categories:
  - Node
tags:
  - [JavaScript, Express.js, 웹 개발]
permalink: /Node/express-params/
toc: true
toc_sticky: true
date: 2024-12-9
last_modified_at: 2024-12-9
---

## JavaScript에서 .params 메서드란?

JavaScript에서 `.params`는 자체적으로 제공되는 메서드가 아니지만, **Express.js**와 같은 웹 프레임워크에서 사용되는 중요한 속성입니다. 특히 Express.js에서는 `.params`를 통해 **라우트 매개변수(route parameters)**를 다루는 데 유용하게 사용됩니다. 

### Express.js에서의 .params 사용

Express.js에서 `req.params`는 URL 경로에서 지정된 매개변수 값을 포함하는 객체입니다. 라우트 정의에서 콜론(`:`)을 사용하여 변수처럼 정의한 경로의 값을 추출할 수 있습니다.

#### 예제 코드

```js
const express = require('express');
const app = express();

// 라우트 정의 (매개변수 이름 :id)
app.get('/user/:id', (req, res) => {
    const userId = req.params.id; // req.params에서 id를 가져옴
    res.send(`User ID: ${userId}`);
});

app.listen(3000, () => {
    console.log('Server is running on port 3000');
});
```

위 코드에서 `/user/:id` 경로를 방문하면, `:id` 부분은 `req.params.id`로 접근할 수 있습니다.

#### 요청과 결과
- **요청:** `GET /user/123`
- **결과:** `User ID: 123`

### .params 사용 시 유의사항

- **URL 매개변수 추출:** `.params`는 URL 경로에서 동적으로 지정된 값을 추출하는 데 사용됩니다.
- **변수명:** 라우트 정의에서 매개변수 이름을 지정할 때, `:`을 사용하여 변수처럼 처리합니다.

### 결론

JavaScript에서 `.params`는 기본적인 JavaScript 메서드가 아니지만, Express.js와 같은 프레임워크에서 매우 중요한 역할을 합니다. URL에서 라우트 매개변수를 추출하는 기능을 통해, 동적인 URL 처리가 가능해집니다. 이를 활용하면 더욱 유연한 웹 서버 개발이 가능합니다.
