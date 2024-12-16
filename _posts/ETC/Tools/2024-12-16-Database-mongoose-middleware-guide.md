---
title: "Mongoose에서 미들웨어의 개요와 활용법"
excerpt: "Mongoose 미들웨어를 활용하여 데이터 유효성 검사, 로깅, 데이터 가공 등의 작업을 효율적으로 처리하는 방법을 알아봅니다."
categories:
  - Tools
  
tags:
  - [JavaScript, Node.js, Mongoose, MongoDB, Backend]
permalink: /Tools/mongoose-middleware-guide/
toc: true
toc_sticky: true
date: 2024-12-16
last_modified_at: 2024-12-16
---

Mongoose의 미들웨어(Middleware)는 MongoDB 데이터베이스와 상호작용할 때 특정 작업 전후에 실행되는 함수로, **"전처리(pre)"**와 **"후처리(post)"**의 두 가지 형태가 있습니다. 이를 활용하면 데이터 유효성 검사, 로깅, 가공, 또는 기타 비즈니스 로직을 쉽게 구현할 수 있습니다.

## Mongoose 미들웨어의 주요 개념

### 1. 미들웨어의 종류
- **Document 미들웨어**: `save`, `remove`, `validate` 등의 작업에 적용됨.
- **Query 미들웨어**: `find`, `findOne`, `updateOne`, `deleteOne` 등의 작업에 적용됨.
- **Aggregate 미들웨어**: `aggregate` 작업에 적용됨.
- **Model 미들웨어**: `insertMany`와 같은 작업에 적용됨.

### 2. Pre와 Post
- **Pre (전처리)**: 특정 작업이 실행되기 전에 실행되는 함수.
- **Post (후처리)**: 특정 작업이 완료된 후에 실행되는 함수.

### 3. 비동기 처리
- 미들웨어 함수는 동기 또는 비동기로 작성할 수 있으며, 비동기 작업의 경우 `next()`를 호출하거나 `Promise`를 반환해야 합니다.

---

## 1. Document 미들웨어

### `save` 전처리 예제
```javascript
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: String,
  password: String,
});

// 저장하기 전에 암호화 처리
userSchema.pre('save', async function (next) {
  if (this.isModified('password')) {
    this.password = await someHashFunction(this.password); // 암호화 함수
  }
  next(); // 다음 단계로 진행
});

const User = mongoose.model('User', userSchema);
```

### `validate` 전처리 예제
```javascript
userSchema.pre('validate', function (next) {
  if (!this.name) {
    throw new Error('Name is required!');
  }
  next();
});
```

### `remove` 후처리 예제
```javascript
userSchema.post('remove', function (doc, next) {
  console.log(`User ${doc.name} has been removed.`);
  next();
});
```

---

## 2. Query 미들웨어

### `find` 전처리 예제
```javascript
userSchema.pre('find', function (next) {
  this.where({ isActive: true }); // 활성화된 사용자만 검색
  next();
});
```

### `findOne` 후처리 예제
```javascript
userSchema.post('findOne', function (doc, next) {
  if (!doc) {
    console.log('No document found!');
  } else {
    console.log(`Found document: ${doc.name}`);
  }
  next();
});
```

---

## 3. Aggregate 미들웨어

### `aggregate` 전처리 예제
```javascript
userSchema.pre('aggregate', function (next) {
  this.pipeline().unshift({ $match: { isActive: true } }); // 활성 사용자만 처리
  next();
});
```

---

## 4. Model 미들웨어

### `insertMany` 전처리 예제
```javascript
userSchema.pre('insertMany', function (next, docs) {
  docs.forEach(doc => {
    doc.createdAt = new Date();
  });
  next();
});
```

---

## 미들웨어 사용 시 주의사항

1. **`next()` 호출 필수**: 전처리 미들웨어에서 `next()`를 호출하지 않으면 다음 작업이 실행되지 않습니다.
2. **비동기 작업**: `async/await` 또는 `Promise`를 사용할 경우 적절히 처리하지 않으면 오류가 발생할 수 있습니다.
3. **this 컨텍스트**: `pre` 미들웨어에서는 `this`가 호출된 문서를 가리키지만, `post` 미들웨어에서는 그렇지 않습니다. `this`를 사용할 때 컨텍스트를 주의해야 합니다.

---

## 응용 예제: 사용자 생성 및 로그인
```javascript
const mongoose = require('mongoose');
const bcrypt = require('bcrypt');

const userSchema = new mongoose.Schema({
  username: { type: String, required: true, unique: true },
  password: { type: String, required: true },
});

// 비밀번호 해시화
userSchema.pre('save', async function (next) {
  if (this.isModified('password')) {
    this.password = await bcrypt.hash(this.password, 10);
  }
  next();
});

// 비밀번호 검증
userSchema.methods.validatePassword = async function (password) {
  return await bcrypt.compare(password, this.password);
};

const User = mongoose.model('User', userSchema);

(async () => {
  const newUser = new User({ username: 'johndoe', password: '123456' });
  await newUser.save();

  const isValid = await newUser.validatePassword('123456');
  console.log('Password is valid:', isValid);
})();
```

---

## 결론

Mongoose 미들웨어는 데이터 처리 로직을 정리하고 재사용성을 높이는 강력한 도구입니다. 데이터 유효성 검사, 데이터 변환, 로깅 등 다양한 작업을 효율적으로 처리할 수 있도록 설계되었습니다. 프로젝트 요구사항에 맞게 적절히 활용해 보세요!

