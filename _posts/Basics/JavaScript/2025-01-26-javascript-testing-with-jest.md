---
title: "Jest: JavaScript 테스트 프레임워크의 개요와 활용법"
excerpt: "Jest의 주요 특징과 장점을 알아보고, 설치 방법부터 간단한 테스트 작성, 통합 테스트 활용법까지 코드 예제와 함께 자세히 설명합니다."
categories:
  - JavaScript
  - Testing
  - Backend
  - Tools
  - Fullstack
  
tags:
  - [Jest, JavaScript, Testing, Node.js, React, 통합 테스트]
permalink: /javascript/testing-with-jest/
toc: true
toc_sticky: true
date: 2025-01-26
last_modified_at: 2025-01-26
---

Jest는 Facebook에서 개발한 JavaScript 테스트 프레임워크로, 빠르고 간단한 설정과 강력한 기능을 제공하여 개발자들 사이에서 널리 사용되고 있습니다. React 애플리케이션을 포함한 다양한 JavaScript/Node.js 프로젝트에 적합하며, 유닛 테스트, 통합 테스트, 그리고 스냅샷 테스트를 손쉽게 작성할 수 있습니다.

이 글에서는 Jest의 주요 특징과 장점, 그리고 실제 프로젝트에서 Jest를 활용하는 방법을 코드 예제와 함께 자세히 소개합니다.

---

## Jest의 주요 특징

1. **간단한 설정**: 별도의 복잡한 설정 없이 바로 테스트를 시작할 수 있습니다.
2. **빠른 실행 속도**: 멀티 쓰레드를 활용하여 테스트를 병렬로 실행합니다.
3. **스냅샷 테스트**: 컴포넌트 출력물을 저장하고 변경 여부를 자동으로 검출합니다.
4. **Mocking 기능**: 함수, 모듈, API 호출 등을 Mocking하여 의존성 없이 테스트를 실행할 수 있습니다.
5. **Watch 모드**: 코드 변경 시 자동으로 관련 테스트를 실행합니다.
6. **강력한 디버깅 툴**: 테스트 실패 시 자세한 에러 메시지와 스택 트레이스를 제공합니다.

---

## Jest 설치 및 기본 설정

### 1. Jest 설치하기
Jest는 NPM 패키지로 제공되므로 다음 명령어로 설치할 수 있습니다:

```bash
npm install --save-dev jest
```

### 2. `package.json` 설정
`package.json` 파일에 Jest 테스트 스크립트를 추가합니다:

```json
{
  "scripts": {
    "test": "jest"
  }
}
```

### 3. 간단한 테스트 작성
테스트 파일은 보통 `.test.js` 또는 `.spec.js` 확장자를 사용하며, 테스트 코드 작성법은 다음과 같습니다:

```javascript
// sum.js
function sum(a, b) {
  return a + b;
}
module.exports = sum;

// sum.test.js
const sum = require('./sum');

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

### 4. 테스트 실행
테스트를 실행하려면 터미널에서 다음 명령어를 입력합니다:

```bash
npm test
```

---

## Jest를 활용한 통합 테스트

Jest는 단순한 유닛 테스트뿐만 아니라 데이터베이스나 API 호출을 포함한 **통합 테스트**도 지원합니다. 다음은 Express.js와 MongoDB를 사용하는 애플리케이션에서 통합 테스트를 설정하는 예제입니다:

### 1. Express.js 라우터 테스트

```javascript
// app.js
const express = require('express');
const app = express();
app.use(express.json());

const items = [];

app.get('/items', (req, res) => {
  res.json(items);
});

app.post('/items', (req, res) => {
  const item = req.body;
  items.push(item);
  res.status(201).json(item);
});

module.exports = app;
```

### 2. 통합 테스트 작성

```javascript
// app.test.js
const request = require('supertest');
const app = require('./app');

describe('Items API', () => {
  it('should return an empty array initially', async () => {
    const res = await request(app).get('/items');
    expect(res.statusCode).toBe(200);
    expect(res.body).toEqual([]);
  });

  it('should add an item', async () => {
    const newItem = { name: 'Test Item' };
    const res = await request(app).post('/items').send(newItem);
    expect(res.statusCode).toBe(201);
    expect(res.body).toEqual(newItem);
  });
});
```

### 3. 테스트 실행 및 결과 확인

```bash
npm test
```

테스트가 성공하면 다음과 같은 결과를 볼 수 있습니다:

```
PASS  ./app.test.js
✓ should return an empty array initially (20ms)
✓ should add an item (10ms)
```

---

## CI/CD 환경에서 Jest 활용하기

Jest는 CircleCI와 같은 CI/CD 파이프라인에서도 쉽게 통합됩니다. CircleCI 설정 파일(`.circleci/config.yml`)에 다음과 같이 Jest 테스트를 추가할 수 있습니다:

```yaml
version: 2.1
jobs:
  test:
    docker:
      - image: circleci/node:16
    steps:
      - checkout
      - run: npm install
      - run: npm test
```

---

## 결론

Jest는 JavaScript 및 Node.js 프로젝트에서 테스트를 작성하고 실행하는 데 있어 매우 강력한 도구입니다. 간단한 설정, 빠른 속도, 그리고 다양한 기능 덕분에 유닛 테스트부터 통합 테스트, CI/CD 파이프라인까지 다양한 시나리오에서 활용할 수 있습니다. 이 글에서 소개한 예제를 기반으로 Jest를 프로젝트에 도입하여 코드 품질을 한 단계 끌어올려 보세요! 😊

