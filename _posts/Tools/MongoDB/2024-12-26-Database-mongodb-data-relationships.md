---
title: "MongoDB 데이터 관계성: Farm과 Product 예제로 배우기"
excerpt: "MongoDB에서 데이터 관계성을 이해하기 위해 Farm과 Product의 예제를 통해 One-to-Many 관계를 설정하고 활용하는 방법을 알아봅니다."
categories:
  - MongoDB
  
tags:
  - [MongoDB, 데이터 관계성, Node.js, Backend]
permalink: /mongodb/mongodb-data-relationships/
toc: true
toc_sticky: true
date: 2024-12-26
last_modified_at: 2024-12-26
---

MongoDB에서 **데이터 관계성**을 설정하고 활용하는 방법을 살펴보겠습니다. 이번 글에서는 **Farm(농장)**과 **Product(제품)** 간의 관계를 예시 코드로 설명합니다.

## 1. Farm과 Product 간의 관계

Farm과 Product 사이에는 **일대다 관계(One-to-Many Relationship)**가 존재합니다:

- 하나의 Farm에는 여러 Product가 속할 수 있습니다.
- 각 Product는 단일 Farm에 소속됩니다.

### 스키마 설계
MongoDB에서는 다음과 같이 스키마를 설계합니다:

- **Farm 모델**:
  - `products` 필드에 Product의 ObjectId를 배열로 저장.
- **Product 모델**:
  - `farm` 필드에 해당 Product가 속한 Farm의 ObjectId를 저장.

이 설계는 데이터의 **참조(Reference)**를 기반으로 관계를 설정합니다. 즉, 실제 데이터는 별도 컬렉션에 저장되고 필요할 때 참조합니다.

---

## 2. 코드로 구현하기

### 제품 추가 폼 렌더링

```javascript
app.get('/farms/:id/products/new', async (req, res) => {
    const { id } = req.params;
    const farm = await Farm.findById(id); // Farm의 ID로 해당 농장 검색
    res.render('products/new', { id, categories, farm }); // 제품 생성 폼에 농장 정보 전달
});
```

- 사용자가 특정 Farm에 제품을 추가하려 할 때, `Farm.findById(id)`로 해당 Farm을 검색합니다.
- 검색된 Farm 정보는 제품 추가 폼으로 전달됩니다.

### 새로운 제품 생성 및 관계 저장

```javascript
app.post('/farms/:id/products', async (req, res) => {
    const { id } = req.params;
    const farm = await Farm.findById(id); // Farm 검색
    const { name, price, category } = req.body;
    const newProduct = new Product({ name, price, category }); // 새로운 Product 생성
    farm.products.push(newProduct); // Farm의 products 배열에 Product 추가
    newProduct.farm = farm; // Product에 해당 Farm 정보 추가
    await farm.save(); // Farm 저장
    await newProduct.save(); // Product 저장
    res.redirect(`/farms/${id}`); // Farm 상세 페이지로 리다이렉트
});
```

1. **Farm과 Product 간 관계 설정:**
    - `farm.products.push(newProduct)`: Farm의 `products` 필드에 Product의 ObjectId 추가.
    - `newProduct.farm = farm`: Product의 `farm` 필드에 Farm의 ObjectId 설정.

2. **저장 순서:**
    - 관계를 메모리에서 설정한 뒤, 각각 `farm.save()`와 `newProduct.save()`로 MongoDB에 저장합니다.

---

## 3. MongoDB에서의 데이터 구조

### Farm 문서 예시:
```json
{
  "_id": "64a3b2d45e07",
  "name": "Happy Farm",
  "location": "California",
  "products": ["64a3b2e76584", "64a3b2e98720"] // Product ObjectId 배열
}
```

### Product 문서 예시:
```json
{
  "_id": "64a3b2e76584",
  "name": "Apple",
  "price": 1.5,
  "category": "Fruit",
  "farm": "64a3b2d45e07" // Farm ObjectId
}
```

---

## 4. 관계 설정 방식의 장점

1. **효율성:**
   - 관련 데이터를 별도 컬렉션에 저장하여 중복을 줄이고 관리 효율성을 높입니다.

2. **유연성:**
   - 필요할 때 ObjectId를 통해 관련 데이터를 참조할 수 있습니다.

3. **확장성:**
   - Farm과 Product의 관계가 많아져도 스키마 변경이 거의 필요 없습니다.

## 5. 데이터 관리 시 주의점

1. **데이터 일관성:**
   - Farm이나 Product 삭제 시 관계 데이터 정리가 필요합니다(예: CASCADE 삭제).

2. **참조 데이터 조회:**
   - 관계 데이터가 필요할 경우 추가 쿼리(`populate`)를 사용해야 합니다.

---

MongoDB에서의 데이터 관계성을 설정하고 활용하는 방법을 이해하면, 효율적이고 확장 가능한 백엔드 시스템을 설계할 수 있습니다. 위의 예제를 기반으로 여러분의 프로젝트에 적합한 데이터 모델을 설계해 보세요!

