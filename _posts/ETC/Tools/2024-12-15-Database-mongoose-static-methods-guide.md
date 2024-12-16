---
title: "Mongoose에서 정적 메서드 활용하기: 모델 기반 데이터 처리의 효율성"
excerpt: "Mongoose의 정적 메서드 정의와 활용 방법을 통해 효율적인 데이터베이스 작업과 코드 재사용성을 높이는 방법을 소개합니다."
categories:
  - Tools
  
tags:
  - [MongoDB, Mongoose, Node.js, Backend, 데이터베이스]
permalink: /Tools/mongoose-static-methods-guide/
toc: true
toc_sticky: true
date: 2024-12-15
last_modified_at: 2024-12-15
---

## Mongoose에서의 정적 메서드란?

Mongoose에서 **정적 메서드**(static methods)는 모델 레벨에서 호출되는 메서드를 정의하는 데 사용됩니다. 이는 모델의 인스턴스가 아닌 **모델 자체에서 호출**할 수 있는 메서드로, 특정 데이터베이스 작업이나 논리를 모듈화하고 재사용할 수 있게 합니다.

---

## 1. 정적 메서드 정의 방법

### 방법 1: `schema.statics`를 사용

```javascript
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: String,
  email: String,
});

// 정적 메서드 정의
userSchema.statics.findByEmail = function (email) {
  return this.findOne({ email }); // `this`는 모델을 참조
};

const User = mongoose.model('User', userSchema);

// 정적 메서드 호출
User.findByEmail('test@example.com').then(user => {
  console.log(user);
});
```

### 방법 2: 스키마 선언 시 `statics` 속성 사용

```javascript
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
}, {
  statics: {
    findByEmail(email) {
      return this.findOne({ email }); // `this`는 모델을 참조
    }
  }
});

const User = mongoose.model('User', userSchema);

User.findByEmail('test@example.com').then(user => {
  console.log(user);
});
```

---

## 2. 정적 메서드의 주요 특징

- **모델 수준에서 호출**: 정적 메서드는 인스턴스가 아닌 모델에서 호출됩니다. 예: `User.findByEmail()`.
- **데이터베이스 작업 모듈화**: 특정 쿼리나 데이터베이스 관련 로직을 한 곳에 정의하여 재사용성을 높입니다.
- **`this` 참조**: 정적 메서드 내부에서 `this`는 현재 모델 객체를 참조합니다.

---

## 3. 정적 메서드의 사용 예시

### (1) 특정 필드로 데이터 검색

```javascript
userSchema.statics.findByName = function (name) {
  return this.find({ name });
};

const User = mongoose.model('User', userSchema);

User.findByName('Alice').then(users => {
  console.log(users); // Alice라는 이름의 모든 사용자
});
```

### (2) 데이터 통계 계산

```javascript
userSchema.statics.countUsers = function () {
  return this.countDocuments();
};

const User = mongoose.model('User', userSchema);

User.countUsers().then(count => {
  console.log(`총 사용자 수: ${count}`);
});
```

### (3) 복잡한 쿼리 캡슐화

```javascript
userSchema.statics.findActiveUsers = function () {
  return this.find({ isActive: true }).sort({ createdAt: -1 });
};

const User = mongoose.model('User', userSchema);

User.findActiveUsers().then(users => {
  console.log(users); // 활성 사용자 목록
});
```

---

## 4. 정적 메서드 vs 인스턴스 메서드

Mongoose에서 정적 메서드는 모델 레벨에서 호출되고, 인스턴스 메서드는 개별 도큐먼트 인스턴스에서 호출됩니다.

| **구분**         | **정적 메서드**                            | **인스턴스 메서드**                   |
|------------------|--------------------------------------------|---------------------------------------|
| **호출 주체**    | 모델 (e.g., `User.methodName()`)            | 도큐먼트 인스턴스 (e.g., `user.methodName()`) |
| **`this` 참조**  | 모델                                       | 현재 도큐먼트 인스턴스               |
| **용도**         | 모델 전체에서 데이터 검색, 통계 계산       | 개별 도큐먼트의 데이터 처리           |

---

## 5. 정적 메서드와 플러그인

Mongoose 플러그인을 작성할 때도 정적 메서드를 활용하여 모델에 공통 기능을 추가할 수 있습니다.

### 예: 플러그인으로 정적 메서드 추가

```javascript
function addTimestampsPlugin(schema) {
  schema.statics.findRecent = function () {
    return this.find().sort({ createdAt: -1 }).limit(10);
  };
}

const postSchema = new mongoose.Schema({
  title: String,
  createdAt: { type: Date, default: Date.now },
});

postSchema.plugin(addTimestampsPlugin);

const Post = mongoose.model('Post', postSchema);

Post.findRecent().then(posts => {
  console.log(posts); // 최근 10개의 게시물
});
```

---

## 6. 참고 사항

- 정적 메서드는 데이터베이스 작업의 캡슐화, 유지보수성, 코드 재사용성을 높이기 위한 강력한 도구입니다.
- 데이터베이스 스키마와 연관된 비즈니스 로직을 처리할 때 적합합니다.
- `async/await`을 사용하면 더 간결하게 비동기 작업을 처리할 수 있습니다.

```javascript
userSchema.statics.findByEmail = async function (email) {
  return await this.findOne({ email });
};
```

이와 같은 방식으로 Mongoose 정적 메서드를 활용하여 데이터 작업을 더욱 체계적으로 관리할 수 있습니다.

