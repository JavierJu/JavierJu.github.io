---
title: "Express에서 express.static 경로 설정의 차이점"
excerpt: "Express에서 정적 파일 경로 설정 시 절대 경로와 상대 경로의 차이를 비교하고, 상황에 따른 권장 사항을 알아봅니다."
categories:
  - Express
  - Node
tags:
  - [JavaScript, Express, Node.js, Backend]
permalink: /node/express-static-path-differences/
toc: true
toc_sticky: true
date: 2024-12-29
last_modified_at: 2024-12-29
---

## Express에서 `express.static` 경로 설정의 차이점

Express에서 정적 파일을 제공할 때 `express.static`을 사용합니다. 이를 설정하는 방법에는 절대 경로와 상대 경로를 사용하는 두 가지 방식이 있는데, 이들의 동작에는 차이가 있습니다. 이 글에서는 그 차이를 설명하고, 상황에 맞는 설정 방법을 제안합니다.

---

### 코드 예제
1. **절대 경로를 사용하는 방식**:
```javascript
const express = require('express');
const path = require('path');
const app = express();

app.use(express.static(path.join(__dirname, '/publics')));
```

2. **상대 경로를 사용하는 방식**:
```javascript
const express = require('express');
const app = express();

app.use(express.static('publics'));
```

---

### 동작의 차이

#### 1. **첫 번째 코드**: 절대 경로 사용
```javascript
app.use(express.static(path.join(__dirname, '/publics')));
```
- **`path.join(__dirname, '/publics')`**:
  - `__dirname`은 현재 파일의 디렉토리 경로를 나타냅니다.
  - `path.join`을 사용해 `publics` 디렉토리를 절대 경로로 지정합니다.
  - Node.js 애플리케이션 실행 위치에 상관없이 항상 정확한 경로를 참조합니다.

#### 2. **두 번째 코드**: 상대 경로 사용
```javascript
app.use(express.static('publics'));
```
- **상대 경로**:
  - `'publics'`는 Node.js 프로세스의 현재 작업 디렉토리(`process.cwd()`)를 기준으로 경로를 설정합니다.
  - 작업 디렉토리가 변경되면 정적 파일 경로가 깨질 수 있습니다.

---

### 장단점 비교

| 방식                        | 장점                                                 | 단점                                                    |
|-----------------------------|------------------------------------------------------|---------------------------------------------------------|
| **절대 경로** (`__dirname`) | 경로의 정확성이 보장됨                              | 코드가 약간 더 복잡함                                   |
| **상대 경로**               | 코드가 간결함                                         | 작업 디렉토리가 변경되면 경로가 깨질 가능성 있음         |

---

### 어떤 방식을 사용할까?

1. **권장 방식**:
   - 대부분의 경우, **절대 경로 방식**을 사용하는 것이 좋습니다. 이는 애플리케이션 실행 위치와 관계없이 경로를 정확히 설정할 수 있기 때문입니다.

2. **특수한 경우**:
   - 상대 경로는 프로젝트 구조가 고정되어 있거나, 간단한 테스트 환경에서만 사용을 고려하세요.

---

### 최종 권장 코드
```javascript
const express = require('express');
const path = require('path');
const app = express();

app.use(express.static(path.join(__dirname, 'publics')));

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```
이와 같이 설정하면, 애플리케이션 실행 위치에 관계없이 안정적으로 정적 파일을 제공할 수 있습니다.

---

### 마무리

Express에서 정적 파일 경로를 설정할 때, 절대 경로와 상대 경로의 차이를 이해하고 올바른 방식을 사용하는 것은 안정적인 애플리케이션 개발의 기초입니다. 특히 실무 환경에서는 항상 절대 경로 방식을 채택하여 경로 관련 문제를 예방하세요.

