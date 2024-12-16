---
title: "Mongoose 스키마 유효성 검사: 데이터 무결성의 핵심"
excerpt: "Mongoose의 스키마를 정의할 때 사용할 수 있는 다양한 유효성 검사 옵션과 커스텀 검증 방법에 대해 알아봅니다."
categories:
  - Tools
tags:
  - [MongoDB, Mongoose, Backend, JavaScript]
permalink: /Tools/mongoose-schema-validation/
toc: true
toc_sticky: true
date: 2024-12-16
last_modified_at: 2024-12-16
---

## Mongoose란 무엇인가?

Mongoose는 MongoDB를 위한 객체 데이터 모델링(ODM) 라이브러리로, 스키마를 사용하여 데이터를 구조화하고 유효성 검사를 적용할 수 있습니다. 이를 통해 데이터의 무결성을 보장하고 코드의 가독성을 높일 수 있습니다.

이 글에서는 Mongoose의 스키마 정의에서 사용할 수 있는 다양한 **유효성 검사(validation)** 옵션과 이를 확장하는 방법에 대해 설명합니다.

---

## Mongoose 스키마 유효성 검사의 종류

Mongoose 스키마에서 지원하는 주요 유효성 검사 옵션은 다음과 같습니다.

### 1. `required`
값이 필수로 존재해야 함을 나타냅니다. 

```javascript
const userSchema = new mongoose.Schema({
  username: { type: String, required: true },
  email: { type: String, required: [true, 'Email is required!'] },
});
```
- **단순 형식**: `required: true`
- **에러 메시지 포함**: `required: [true, '필드가 비어 있습니다.']`

### 2. `minlength`와 `maxlength`
문자열의 최소 및 최대 길이를 제한합니다.

```javascript
const productSchema = new mongoose.Schema({
  name: { type: String, minlength: 3, maxlength: 50 },
});
```

### 3. `min`와 `max`
숫자 데이터의 최소값 및 최대값을 설정합니다.

```javascript
const orderSchema = new mongoose.Schema({
  quantity: { type: Number, min: 1, max: 100 },
});
```

### 4. `match`
정규식을 이용해 문자열 값의 형식을 검증합니다.

```javascript
const userSchema = new mongoose.Schema({
  email: { type: String, match: /.+\@.+\..+/ },
});
```

### 5. `enum`
값이 지정된 배열 중 하나인지 확인합니다.

```javascript
const taskSchema = new mongoose.Schema({
  status: { type: String, enum: ['pending', 'completed', 'failed'] },
});
```

### 6. `default`
기본값을 설정합니다.

```javascript
const userSchema = new mongoose.Schema({
  role: { type: String, default: 'user' },
});
```

---

## 커스텀 유효성 검사

Mongoose는 스키마 정의에 사용자 정의 유효성 검사를 추가할 수 있는 `validate` 옵션을 제공합니다.

### 1. 간단한 커스텀 검증 함수

```javascript
const userSchema = new mongoose.Schema({
  age: {
    type: Number,
    validate: {
      validator: function (value) {
        return value >= 18;
      },
      message: 'Age must be at least 18.',
    },
  },
});
```

### 2. 비동기 검증
비동기 함수가 필요할 경우 `isAsync` 또는 Promise 기반 검증을 사용할 수 있습니다.

```javascript
const userSchema = new mongoose.Schema({
  username: {
    type: String,
    validate: {
      validator: async function (value) {
        const user = await this.constructor.findOne({ username: value });
        return !user; // 이미 존재하는 이름은 허용하지 않음
      },
      message: 'Username already exists.',
    },
  },
});
```

### 3. 사용자 정의 메시지
에러 메시지를 동적으로 생성할 수 있습니다.

```javascript
const productSchema = new mongoose.Schema({
  price: {
    type: Number,
    validate: {
      validator: function (value) {
        return value > 0;
      },
      message: (props) => `${props.value} is not a valid price!`,
    },
  },
});
```

---

## Mongoose 유효성 검사 활용 팁

1. **트랜잭션과 결합**: 유효성 검사를 데이터베이스 트랜잭션과 결합하여 데이터 무결성을 더욱 강화합니다.
2. **에러 핸들링**: Mongoose의 `ValidationError` 객체를 활용하여 클라이언트에 구체적인 에러 메시지를 전달합니다.
3. **복잡한 유효성 검사**: 간단한 필드 수준 유효성 검사로 해결되지 않는 경우, 미들웨어 또는 별도의 비즈니스 로직으로 처리합니다.

---

## 결론

Mongoose의 스키마 유효성 검사는 데이터베이스에서 불필요한 데이터 오염을 방지하고 응용 프로그램의 신뢰성을 높이는 데 중요한 역할을 합니다. 기본 제공되는 옵션 외에도 커스텀 검증을 통해 다양한 요구 사항을 충족할 수 있으며, 이를 적절히 활용하여 데이터 무결성을 보장하세요.

