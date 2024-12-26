---
title: "Mongoose 인스턴스 메서드의 이해와 활용"
excerpt: "Mongoose의 인스턴스 메서드를 설정하고 사용하는 방법, 그리고 비즈니스 로직에 캡슐화된 기능을 구현하는 사례를 살펴봅니다."
categories:
  - Node
permalink: /Node/mongoose-instance-methods/
toc: true
toc_sticky: true
date: 2024-12-15
last_modified_at: 2024-12-15
---

Mongoose에서 **인스턴스 메서드(instance methods)**는 모델의 개별 문서(document)에서 호출할 수 있는 사용자 정의 메서드입니다. 이를 사용하면 데이터베이스 문서와 직접적으로 관련된 로직을 깔끔하게 캡슐화할 수 있습니다.

---

## 1. 인스턴스 메서드란?

인스턴스 메서드는 데이터베이스에서 반환된 특정 문서(document)에서만 사용할 수 있는 메서드입니다. 이 메서드는 일반적으로 데이터 조작, 상태 변경, 또는 특정 문서와 관련된 비즈니스 로직을 수행하는 데 사용됩니다.

---

## 2. 인스턴스 메서드 설정 방법

### Step 1: Schema 정의

인스턴스 메서드를 추가하려면 `Schema.methods` 객체를 사용합니다.

```javascript
const mongoose = require('mongoose');

const productSchema = new mongoose.Schema({
    name: String,
    price: Number,
    onSale: { type: Boolean, default: false }
});

// **여기서 인스턴스 메서드를 정의**
productSchema.methods.toggleOnSale = function () {
    this.onSale = !this.onSale; // 상태를 반전
    return this.save(); // 변경 사항 저장
};

// **메서드에 파라미터를 활용한 예시**
productSchema.methods.addDiscount = function (discount) {
    this.price = this.price - discount;
    return this.save();
};

// 모델 생성
const Product = mongoose.model('Product', productSchema);
```

---

### Step 2: 인스턴스 메서드 사용

생성된 문서(document)에서 메서드를 호출할 수 있습니다.

```javascript
const run = async () => {
    await mongoose.connect('mongodb://127.0.0.1:27017/shopApp');
    
    // 새로운 상품 생성
    const product = new Product({ name: 'Bike', price: 500 });
    await product.save();

    // **인스턴스 메서드 사용**
    console.log('Before toggle:', product);
    await product.toggleOnSale();
    console.log('After toggle:', product);

    // 할인 적용
    await product.addDiscount(50);
    console.log('After discount:', product);
};

run();
```

---

## 3. 인스턴스 메서드의 사용 상황

### (1) 데이터 상태 변경
- 특정 문서의 필드를 업데이트하고 저장해야 할 때 사용합니다.
- **예:** 할인 적용, 상태 전환, 카운터 증가 등.

### (2) 비즈니스 로직 캡슐화
- 문서와 관련된 특정 로직을 코드에서 재사용하기 쉽게 만들 수 있습니다.
- **예:** 특정 조건에서 데이터 유효성 검사와 업데이트를 동시에 수행.

### (3) 데이터와의 상호작용
- 하나의 문서를 기준으로 데이터베이스와 상호작용해야 하는 경우.
- **예:** 현재 문서의 특정 값에 따라 연관된 데이터를 조회하거나 업데이트.

---

## 4. 주의점

### (1) 메서드 정의 순서
- **모델을 생성하기 전에** `Schema.methods`에 메서드를 정의해야 합니다. 모델 생성 이후에는 메서드가 적용되지 않습니다.

### (2) 메서드의 `this`
- 인스턴스 메서드 내부의 `this`는 해당 문서를 참조합니다. 따라서 화살표 함수는 사용하지 않아야 합니다.

```javascript
productSchema.methods.badMethod = () => {
    console.log(this); // `this`가 undefined가 됩니다.
};
```

### (3) 문서를 저장해야 반영
- 메서드에서 변경된 문서의 필드는 데이터베이스에 저장(`save()`)해야 반영됩니다. 저장하지 않으면 메모리에서만 변경됩니다.

---

## 5. 인스턴스 메서드 vs 정적 메서드

### 인스턴스 메서드
- 특정 문서와 관련된 작업에 사용됩니다.
- **예:** 상태 변경, 필드 업데이트 등.

### 정적 메서드 (Static Methods)
- 모델 전체와 관련된 작업에 사용됩니다. 특정 문서가 아닌 모델 자체에서 호출 가능합니다.
- `Schema.statics`를 사용해 정의합니다.

```javascript
productSchema.statics.findByCategory = function (category) {
    return this.find({ categories: category });
};
```

#### 사용 예:

```javascript
const products = await Product.findByCategory('electronics');
```

---

## 6. 인스턴스 메서드의 활용 사례

### (1) 상태 토글
```javascript
productSchema.methods.toggleAvailability = function () {
    this.available = !this.available;
    return this.save();
};
```

### (2) 데이터 누적
```javascript
productSchema.methods.incrementStock = function (amount) {
    this.stock += amount;
    return this.save();
};
```

### (3) 복합 로직 처리
```javascript
productSchema.methods.applyDiscountIfOnSale = function (discount) {
    if (this.onSale) {
        this.price = this.price - discount;
    }
    return this.save();
};
```

---

## 7. 요약

- `Schema.methods`로 인스턴스 메서드를 정의하여 문서와 관련된 로직을 캡슐화할 수 있습니다.
- 메서드 정의는 모델 생성 전에 수행해야 하며, 메서드 내부에서 `this`는 해당 문서를 참조합니다.
- 주로 **상태 변경, 데이터 업데이트, 비즈니스 로직**을 처리하는 데 사용됩니다.
- 정적 메서드와 함께 사용하면 모델의 문서 관리와 비즈니스 로직을 명확히 분리할 수 있습니다.

