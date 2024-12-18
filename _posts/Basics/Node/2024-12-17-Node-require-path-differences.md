---
title: "Node.js require() 경로 이해하기: './categories'와 '../categories' 차이"
excerpt: "Node.js에서 require() 함수로 모듈을 불러올 때, 상대 경로의 차이인 './'와 '../'의 사용법과 차이를 이해해 봅니다."
categories:
  - Node
tags:
  - [Node.js, JavaScript, 모듈, 경로]
permalink: /Node/require-path-differences/
toc: true
toc_sticky: true
date: 2024-12-17
last_modified_at: 2024-12-17
---

Node.js에서 `require()` 함수는 외부 모듈이나 파일을 가져오는 데 사용됩니다. 하지만 경로를 지정할 때 `./`와 `../`의 사용법을 혼동하는 경우가 많습니다. 이 글에서는 두 방식의 차이를 설명하고, 적절히 사용하는 방법을 알아봅니다.

---

### 1. `const categories = require('./categories');`
- `./categories`는 **현재 파일과 같은 디렉터리**에 위치한 `categories` 파일(또는 디렉터리)을 참조합니다.
- `./`는 현재 디렉터리를 나타냅니다.

#### 예시 디렉터리 구조:

```
project/
├── categories.js
└── src/
    └── app.js
```

- `app.js`에서 `categories.js`를 불러오려면:

  ```javascript
  const categories = require('../categories');
  ```

- 반면, 프로젝트 루트에서 `categories.js`를 불러오려면:

  ```javascript
  const categories = require('./categories');
  ```

---

### 2. `const categories = require('../categories');`
- `../categories`는 **현재 파일의 상위 디렉터리**에 위치한 `categories` 파일(또는 디렉터리)을 참조합니다.
- `../`는 상위 디렉터리를 나타냅니다.

#### 예시 디렉터리 구조:

```
project/
├── categories.js
└── src/
    └── utils.js
```

- `utils.js`에서 `categories.js`를 불러오려면:

  ```javascript
  const categories = require('../categories');
  ```

---

### 주요 차이점 요약
- **`./categories`**: 현재 디렉터리에 있는 파일이나 디렉터리를 참조합니다.
- **`../categories`**: 상위 디렉터리에 있는 파일이나 디렉터리를 참조합니다.

---

### Node.js에서 `require()` 사용 시 유의 사항
1. **상대 경로 기준**:
   - 경로는 현재 파일의 위치를 기준으로 작성해야 합니다.
2. **절대 경로 사용**:
   - 프로젝트가 커지면 상대 경로 사용이 복잡해질 수 있습니다. 이 경우 절대 경로를 설정하는 것이 더 효율적입니다. 이를 위해 `module-alias` 패키지를 사용할 수 있습니다.
3. **디렉터리 구조 이해**:
   - 디렉터리 구조를 명확히 이해하면 모듈 경로 설정이 쉬워집니다.

---

### 마무리
Node.js의 `require()` 함수에서 경로를 지정할 때 `./`와 `../`의 차이를 이해하는 것은 효율적인 코드 작성을 위한 기본입니다. 디렉터리 구조를 명확히 하고, 적절한 경로를 사용하는 습관을 들이면 프로젝트 관리가 훨씬 쉬워질 것입니다.

