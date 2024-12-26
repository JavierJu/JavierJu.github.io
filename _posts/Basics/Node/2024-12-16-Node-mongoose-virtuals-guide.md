---
title: "Mongoose 가상 필드: Getter와 Setter 활용법"
excerpt: "Mongoose의 가상 필드(Virtuals)를 정의하고, Getter와 Setter를 활용하여 데이터를 효과적으로 관리하는 방법을 알아봅니다."
categories:
  - Node
tags:
  - [JavaScript, Mongoose, MongoDB, ODM, Backend]
permalink: /Node/mongoose-virtuals-guide/
toc: true
toc_sticky: true
date: 2024-12-16
last_modified_at: 2024-12-16
---

# Mongoose의 가상 필드: Getter와 Setter 활용법

Mongoose는 Node.js 환경에서 MongoDB와 상호작용하기 위한 객체 데이터 모델링(ODM) 라이브러리입니다. MongoDB는 기본적으로 비정형 데이터베이스이지만, Mongoose는 스키마(Schema)를 정의하여 데이터를 구조화하고 관리할 수 있게 해줍니다.

Mongoose에서 **가상(Virtuals)**은 스키마에 실제로 데이터베이스에 저장되지 않는 필드를 정의할 수 있는 기능입니다. 이 가상 필드는 계산된 값(computed value)을 반환하거나 데이터를 처리할 때 유용하게 사용할 수 있습니다.

---

## **가상(Virtual) 필드**
가상 필드는 MongoDB에 저장되지 않으며, 주로 데이터의 표현 또는 조작에 사용됩니다. 예를 들어, `fullName` 필드를 정의하여 `firstName`과 `lastName`을 조합하여 반환할 수 있습니다.

### **기본 사용법**
```javascript
const mongoose = require('mongoose');
const { Schema } = mongoose;

// User 스키마 정의
const userSchema = new Schema({
  firstName: String,
  lastName: String,
});

// 가상 필드 정의
userSchema.virtual('fullName').get(function () {
  return `${this.firstName} ${this.lastName}`;
});

// 모델 생성
const User = mongoose.model('User', userSchema);

// 예제
const user = new User({ firstName: 'John', lastName: 'Doe' });
console.log(user.fullName); // "John Doe"
```

---

## **Setter와 Getter**

가상 필드는 `getter`와 `setter`를 모두 지원합니다.

- **Getter**는 데이터를 읽을 때 값을 계산하거나 변환하는 데 사용됩니다.
- **Setter**는 데이터를 설정할 때 계산하거나 처리하는 데 사용됩니다.

### **Getter & Setter 예제**
```javascript
// 가상 필드에 Getter와 Setter 정의
userSchema.virtual('fullName')
  .get(function () {
    return `${this.firstName} ${this.lastName}`;
  })
  .set(function (value) {
    const [firstName, lastName] = value.split(' ');
    this.firstName = firstName;
    this.lastName = lastName;
  });

// 모델 생성
const User = mongoose.model('User', userSchema);

// 데이터 설정 및 출력
const user = new User();
user.fullName = 'Jane Doe'; // Setter 호출
console.log(user.firstName); // "Jane"
console.log(user.lastName);  // "Doe"
console.log(user.fullName);  // "Jane Doe" (Getter 호출)
```

---

## **스키마 옵션: `toJSON` 및 `toObject`**
가상 필드는 기본적으로 JSON 직렬화 또는 객체 변환에서 포함되지 않습니다. 가상 필드를 직렬화 결과에 포함하려면 스키마에 `toJSON` 또는 `toObject` 옵션을 설정해야 합니다.

### **toJSON 설정**
```javascript
const userSchema = new Schema(
  {
    firstName: String,
    lastName: String,
  },
  {
    toJSON: { virtuals: true }, // 가상 필드를 JSON 출력에 포함
  }
);

userSchema.virtual('fullName').get(function () {
  return `${this.firstName} ${this.lastName}`;
});

const User = mongoose.model('User', userSchema);

const user = new User({ firstName: 'Alice', lastName: 'Smith' });
console.log(user.toJSON());
// { firstName: 'Alice', lastName: 'Smith', fullName: 'Alice Smith' }
```

---

## **가상 필드의 주요 특징**
1. **데이터베이스에 저장되지 않음**: MongoDB의 실제 데이터에는 영향을 주지 않음.
2. **Getter/Setter 지원**: 계산된 값 반환 및 입력 데이터 처리 가능.
3. **toJSON, toObject 옵션**: 직렬화 시 포함 여부를 제어 가능.
4. **복잡한 데이터 처리 가능**: 다른 필드들을 조합하거나 별도로 정의된 논리에 따라 동작.

---

## **실제 사용 사례**
1. **Full Name 계산**:
   - `firstName`과 `lastName` 조합.
2. **가공된 데이터 출력**:
   - 나이 계산 (예: `birthDate` 필드에서 `age`를 계산).
3. **상태 필드 표현**:
   - 계산된 논리 상태를 표현하는 데 사용 (예: `isActive`).
4. **주소 포맷팅**:
   - 여러 주소 필드를 하나로 조합해 출력.

---

## **주의사항**
- 가상 필드는 데이터베이스 쿼리 시 사용되지 않습니다. 예를 들어, `fullName`으로 직접 MongoDB 쿼리를 작성할 수는 없습니다.
- 복잡한 연산은 가상 필드에서 구현하지 않고 별도의 메서드로 분리하는 것이 좋습니다.

Mongoose의 가상 필드를 적절히 사용하면 데이터 조작 및 표현이 훨씬 유연해집니다!

