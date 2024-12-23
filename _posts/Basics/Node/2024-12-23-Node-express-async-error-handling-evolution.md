---
title: "Express의 비동기 오류 처리: 유틸리티 함수와 최신 접근법"
excerpt: "Express 4에서 유용했던 비동기 오류 처리 유틸리티 함수와 Express 5 및 express-async-errors 라이브러리를 활용한 더 간결한 접근 방법을 비교합니다."
categories:
  - Node
tags:
  - [Express, Node.js, Backend, 비동기 처리, 오류 처리]
permalink: /Node/express-async-error-handling-evolution/
toc: true
toc_sticky: true
date: 2024-12-23
last_modified_at: 2024-12-23
---

Express 애플리케이션에서 비동기 오류 처리는 개발자가 흔히 마주하는 문제 중 하나입니다. Express 4에서 널리 사용되던 유틸리티 함수들과 최신 Express 5 또는 `express-async-errors` 라이브러리를 활용한 방법을 비교하며, 왜 최신 접근법이 더 유리한지 살펴보겠습니다.

---

## 1. **Express 4와 비동기 오류 처리 유틸리티 함수**

Express 4에서는 비동기 핸들러에서 발생하는 오류를 자동으로 처리하지 못했기 때문에, 유틸리티 함수가 자주 사용되었습니다. 아래는 대표적인 예입니다.

### **(1) wrapAsync 함수**

```javascript
function wrapAsync(fn) {
    return function (req, res, next) {
        fn(req, res, next).catch(e => next(e));
    };
}

// 사용 예시
app.get('/products', wrapAsync(async (req, res, next) => {
    const { category } = req.query;
    const products = category
        ? await Product.find({ category })
        : await Product.find({});
    res.render('products/index', { products, category: category || 'All' });
}));
```

- `wrapAsync`는 비동기 함수(`fn`)를 감싸고, `.catch()`를 통해 발생한 오류를 `next(e)`로 Express의 오류 처리 미들웨어로 전달합니다.

### **(2) asyncHandler 함수**

```javascript
const asyncHandler = (fn) => (req, res, next) => {
    Promise.resolve(fn(req, res, next)).catch(next);
};

// 사용 예시
app.get('/async-route', asyncHandler(async (req, res) => {
    const result = await someAsyncFunction();
    res.json(result);
}));
```

- `asyncHandler`는 `Promise.resolve()`를 활용해 비동기 핸들러의 반환 값을 처리하며, `.catch(next)`를 통해 오류를 전달합니다.
- `wrapAsync`와 기능적으로 동일하지만, 코드 스타일이 약간 더 간결합니다.

---

## 2. **express-async-errors 라이브러리를 활용한 간소화**

`express-async-errors` 라이브러리를 사용하면, 위와 같은 유틸리티 함수가 필요 없어집니다.

### **설치 및 사용법**

```bash
npm install express-async-errors
```

```javascript
require('express-async-errors');
const express = require('express');
const app = express();

app.get('/async-route', async (req, res) => {
    const result = await someAsyncFunction(); // 오류 발생 시 자동으로 처리됨
    res.json(result);
});

// 오류 처리 미들웨어
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ error: 'Something went wrong!' });
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

- 이 라이브러리는 모든 비동기 핸들러에서 발생한 오류를 자동으로 감지하고, Express의 오류 처리 미들웨어로 전달합니다.
- 유틸리티 함수 없이도 안전한 비동기 오류 처리가 가능합니다.

---

## 3. **Express 5: 기본 제공 비동기 오류 처리**

Express 5는 `async/await`로 작성된 핸들러에서 발생하는 오류를 자동으로 처리하는 기능을 제공합니다.

### **Express 5 예제**

```javascript
const express = require('express');
const app = express();

app.get('/async-route', async (req, res) => {
    const result = await someAsyncFunction(); // 오류 발생 시 자동으로 처리됨
    res.json(result);
});

// 오류 처리 미들웨어
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ error: 'Something went wrong!' });
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

- 비동기 핸들러에서 `try-catch`를 사용하지 않아도 Express가 자동으로 오류를 감지하고 처리합니다.
- `express-async-errors`와 동일한 기능을 프레임워크 차원에서 지원합니다.

---

## 4. **유틸리티 함수는 이제 필요 없을까?**

### **언제 유틸리티 함수가 필요하지 않은가?**
1. **`express-async-errors`를 사용하는 경우**:
   - Express 4에서도 비동기 오류 처리를 자동화할 수 있으므로 유틸리티 함수가 불필요합니다.

2. **Express 5로 업그레이드한 경우**:
   - 비동기 오류 처리가 기본적으로 지원되므로, 별도의 유틸리티 함수 없이도 안전한 코드 작성이 가능합니다.

### **유틸리티 함수가 유용한 경우**
- Express 4를 사용하며, 추가 라이브러리를 설치하지 않는 환경에서는 여전히 유틸리티 함수가 필요할 수 있습니다.

---

## 5. **최신 접근법의 장점**

### **코드 간소화**
- 추가적인 유틸리티 함수 없이도 비동기 오류 처리가 가능해져 코드가 간결해집니다.

### **유지보수 용이성**
- 비동기 오류 처리를 위해 매번 유틸리티 함수를 작성하거나 호출할 필요가 없으므로, 유지보수가 쉬워집니다.

### **일관된 처리**
- 동기 및 비동기 핸들러 모두 동일한 방식으로 오류를 처리할 수 있습니다.

---

## 6. **결론**

Express 5 또는 `express-async-errors`를 사용하는 경우, 비동기 오류 처리가 기본적으로 제공되므로 추가적인 유틸리티 함수가 필요 없습니다. 이는 코드의 간결성과 유지보수성을 크게 향상시키며, 최신 Express 애플리케이션 개발의 Best Practice로 자리 잡고 있습니다.

