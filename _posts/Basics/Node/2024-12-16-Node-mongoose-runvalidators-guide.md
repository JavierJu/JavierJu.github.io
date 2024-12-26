---
title: "Mongoose에서 runValidators의 역할과 사용법"
excerpt: "Mongoose에서 업데이트 시 스키마 유효성 검사를 적용하는 runValidators 옵션의 동작 원리와 코드 예제를 알아봅니다."
categories:
  - Node
tags:
  - [Mongoose, MongoDB, JavaScript, Backend, Data Validation]
permalink: /Node/mongoose-runvalidators-guide/
toc: true
toc_sticky: true
date: 2024-12-16
last_modified_at: 2024-12-16
---

## Mongoose에서 `runValidators`란?

Mongoose에서 `runValidators` 옵션은 **`update` 또는 `findOneAndUpdate`**와 같은 메서드를 사용할 때, **스키마에 정의된 유효성 검사를 강제로 실행**하도록 해주는 중요한 옵션입니다.

기본적으로 `update`와 `findOneAndUpdate`는 스키마의 유효성 검사를 수행하지 않습니다. 따라서 데이터가 스키마의 유효성 검사를 통과하지 않더라도 업데이트가 적용될 수 있습니다. 하지만 `runValidators: true`를 설정하면, 유효성 검사가 적용되어 유효하지 않은 데이터는 데이터베이스에 저장되지 않고 에러가 발생합니다.

---

## 코드에서 `runValidators: true` 역할

### 코드 예제:
```javascript
Product.findOneAndUpdate(
  { name: 'Air Pump' },               // 조건: name이 'Air Pump'인 문서를 찾음
  { price: -15.50 },                  // 업데이트: price를 -15.50으로 설정
  { new: true, runValidators: true }  // 옵션: 업데이트된 문서를 반환, 유효성 검사 실행
)
  .then(data => {
    console.log('IT WORKED!');
    console.log(data);
  })
  .catch(err => {
    console.log('OH NO ERROR');
    console.log(err);
  });
```

### 옵션 설명:
1. **조건**: `name` 필드가 `'Air Pump'`인 문서를 찾습니다.
2. **업데이트**: 해당 문서의 `price` 필드를 `-15.50`으로 업데이트하려고 합니다.
3. **옵션**:
   - `new: true`: 업데이트된 문서를 반환합니다. 기본값은 `false`이며, 기존 문서를 반환합니다.
   - `runValidators: true`: 스키마에 정의된 유효성 검사를 실행합니다.

---

## 예시와 에러 처리

### 스키마:

```javascript
const productSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true
  },
  price: {
    type: Number,
    min: 0   // 가격은 0 이상이어야 함
  }
});

const Product = mongoose.model('Product', productSchema);
```

### 실행 결과:
1. `price`가 `-15.50`으로 설정되려고 하지만, 스키마에서 `price`는 `0` 이상이어야 한다고 정의되어 있습니다.
2. `runValidators: true`가 설정되어 있으므로, 유효성 검사가 실행되고 실패합니다.
3. `catch` 블록에서 에러 메시지가 출력됩니다:

   ```javascript
   OH NO ERROR
   ValidationError: Product validation failed: price: Path `price` (-15.5) is less than minimum allowed value (0).
   ```

---

## 주의할 점

- `runValidators`는 **`update` 연산**에서만 적용됩니다. 기본적으로는 유효성 검사를 실행하지 않습니다.
- 유효성 검사는 **업데이트된 필드에 대해서만 적용**됩니다.
  - 다른 필드가 검사를 요구하더라도, 해당 필드를 업데이트하지 않으면 검사가 수행되지 않습니다.
- `runValidators`를 사용하려면 반드시 옵션으로 명시해야 합니다. 그렇지 않으면 스키마 검증이 건너뛰어집니다.

---

## 결론

`runValidators: true`는 업데이트 시 유효성 검사를 보장하기 위한 옵션입니다. 기본 동작에서는 Mongoose가 업데이트 중 스키마 검증을 하지 않기 때문에, 데이터 무결성을 유지하려면 항상 이 옵션을 사용하는 것이 좋습니다.

