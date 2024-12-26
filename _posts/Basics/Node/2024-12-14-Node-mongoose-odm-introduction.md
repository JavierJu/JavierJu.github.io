---
title: "Mongoose ODM: Node.js와 MongoDB의 데이터 관리 솔루션"
excerpt: "Mongoose ODM을 사용한 MongoDB 데이터 모델링과 CRUD 작업 방법을 알아보고, 주요 기능과 장단점을 탐구합니다."
categories:
  - Node
permalink: /Node/mongoose-odm-introduction/
toc: true
toc_sticky: true
date: 2024-12-14
last_modified_at: 2024-12-14
---

## Mongoose란 무엇인가?

Mongoose는 Node.js 환경에서 MongoDB와 상호작용할 수 있도록 도와주는 **Object Data Modeling (ODM)** 라이브러리입니다. Mongoose를 사용하면 MongoDB의 데이터를 객체처럼 다룰 수 있고, 데이터 모델링, 유효성 검사, 쿼리 빌딩, 미들웨어와 같은 다양한 기능을 제공합니다.

---

## 주요 기능과 개념

### 1. **스키마(Schema) 정의**
Mongoose에서 데이터 구조를 정의하려면 스키마를 사용합니다. 스키마는 MongoDB의 Collection에 저장될 문서(Document)의 구조를 지정합니다.

```javascript
const mongoose = require('mongoose');

// User 스키마 정의
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  age: { type: Number, min: 0 },
  createdAt: { type: Date, default: Date.now }
});
```

### 2. **모델(Model) 생성**
스키마로부터 모델을 생성하면 해당 모델을 통해 MongoDB의 컬렉션에 데이터를 CRUD(생성, 읽기, 수정, 삭제)할 수 있습니다.

```javascript
const User = mongoose.model('User', userSchema);
```

### 3. **데이터 생성**
모델 인스턴스를 만들어 데이터를 저장할 수 있습니다.

```javascript
const newUser = new User({
  name: 'John Doe',
  email: 'john@example.com',
  age: 25
});

newUser.save()
  .then(user => console.log('User saved:', user))
  .catch(err => console.error('Error saving user:', err));
```

### 4. **데이터 조회**
데이터를 조회하려면 Mongoose의 다양한 쿼리 메서드를 사용할 수 있습니다.

```javascript
// 모든 사용자 조회
User.find()
  .then(users => console.log('All users:', users))
  .catch(err => console.error(err));

// 특정 조건으로 조회
User.findOne({ email: 'john@example.com' })
  .then(user => console.log('Found user:', user))
  .catch(err => console.error(err));
```

### 5. **데이터 수정**
데이터를 수정하려면 `updateOne`, `updateMany`, `findByIdAndUpdate` 등을 사용할 수 있습니다.

```javascript
User.updateOne({ email: 'john@example.com' }, { $set: { age: 26 } })
  .then(result => console.log('Update result:', result))
  .catch(err => console.error(err));
```

### 6. **데이터 삭제**
`deleteOne`, `deleteMany`, `findByIdAndDelete` 등을 사용해 데이터를 삭제할 수 있습니다.

```javascript
User.deleteOne({ email: 'john@example.com' })
  .then(result => console.log('Delete result:', result))
  .catch(err => console.error(err));
```

---

## Mongoose의 주요 기능

### 1. **유효성 검사(Validation)**
스키마 레벨에서 데이터의 유효성을 검사할 수 있습니다. 예를 들어, 필수값, 최소값/최대값, 정규식 등을 설정할 수 있습니다.

```javascript
const productSchema = new mongoose.Schema({
  name: { type: String, required: true },
  price: { type: Number, min: 0 }
});
```

### 2. **미들웨어(Middleware)**
Mongoose는 문서의 생명 주기 동안 특정 이벤트 전에 미들웨어를 실행할 수 있습니다. 예를 들어, 데이터를 저장하기 전에 특정 작업을 수행할 수 있습니다.

```javascript
userSchema.pre('save', function(next) {
  console.log('Before saving user:', this);
  next();
});
```

### 3. **가상 필드(Virtuals)**
가상 필드는 실제 MongoDB 문서에 저장되지는 않지만, 계산된 데이터를 반환하는 데 사용됩니다.

```javascript
userSchema.virtual('fullName').get(function() {
  return `${this.firstName} ${this.lastName}`;
});
```

### 4. **인덱스(Index)**
Mongoose를 통해 컬렉션 필드에 인덱스를 설정할 수 있습니다. 이는 조회 성능을 향상시키는 데 유용합니다.

```javascript
userSchema.index({ email: 1 });
```

### 5. **플러그인(Plugin)**
Mongoose는 스키마에 플러그인을 추가하여 확장성을 제공합니다.

```javascript
function timestamp(schema) {
  schema.add({ createdAt: Date, updatedAt: Date });

  schema.pre('save', function(next) {
    this.updatedAt = Date.now();
    if (!this.createdAt) {
      this.createdAt = Date.now();
    }
    next();
  });
}

userSchema.plugin(timestamp);
```

---

## Mongoose를 사용하는 단계

1. **MongoDB 연결**
   Mongoose를 사용하려면 먼저 MongoDB와 연결해야 합니다.

   ```javascript
   const mongoose = require('mongoose');
   mongoose.connect('mongodb://127.0.0.1:27017/mydb')
    .then(() => console.log('MongoDB connected'));
    .catch(err => console.error('MongoDB connection error:', err));
   ```

2. **스키마 및 모델 정의**
   스키마를 정의하고 모델을 생성합니다.

3. **CRUD 작업 수행**
   모델을 사용하여 데이터를 생성, 읽기, 수정, 삭제 작업을 수행합니다.

---

## Mongoose의 장점
- MongoDB 데이터를 객체지향적으로 관리 가능
- 내장된 유효성 검사 및 미들웨어 기능
- 쿼리 메서드 및 플러그인을 통한 확장성 제공
- 간단한 API로 빠른 개발 가능

## 단점
- ODM 라이브러리로 인해 MongoDB의 일부 기능 사용이 제한될 수 있음
- 복잡한 쿼리에서는 Mongoose의 추상화가 오히려 불편할 수 있음
- 추가 학습 곡선 필요

---

Mongoose는 MongoDB와 Node.js 애플리케이션 간의 인터페이스를 단순화하고 강력한 데이터 관리 기능을 제공합니다. 대규모 애플리케이션에서 데이터를 구조화하고 유효성을 보장하는 데 매우 유용합니다.

