---
title: "Mongoose 미들웨어의 활용: 농장과 상품 삭제 예제"
excerpt: "Mongoose의 post 미들웨어를 활용하여 참조된 데이터를 자동으로 삭제하는 방법을 농장과 상품 관계를 중심으로 설명합니다."
categories:
  - Node
tags:
  - [Mongoose, Middleware, MongoDB, Backend, Node.js]
permalink: /Node/mongoose-middleware-example/
toc: true
toc_sticky: true
date: 2024-12-26 
last_modified_at: 2024-12-26 
---

Mongoose의 미들웨어를 사용하면 데이터베이스 작업 전에 또는 후에 추가 작업을 수행할 수 있습니다. 이번 글에서는 특정 컬렉션에서 데이터를 삭제할 때, 관련된 데이터를 자동으로 처리하는 **post 미들웨어**를 사용한 예제를 살펴보겠습니다.

## 예제 코드

### farm.js
```javascript
farmSchema.post('findOneAndDelete', async function (farm) {
    if (farm.products.length) {
        const res = await Product.deleteMany({ _id: { $in: farm.products } })
        console.log(res);
    }
});
```

### index.js
```javascript
//Delete farm
app.delete('/farms/:id', async (req, res) => {
    const { id } = req.params;
    const farm = await Farm.findByIdAndDelete(id);
    res.redirect('/farms');
});
```

## 코드 설명

### 1. **farm.js**: 미들웨어 설정
- `farmSchema.post('findOneAndDelete', ...)`:
  - `findOneAndDelete` 메서드가 실행된 **후**에 실행되는 `post` 미들웨어입니다.

- **작동 방식**:
  1. 삭제된 농장 문서(`farm`)가 `farm` 인수로 전달됩니다.
  2. 삭제된 농장에 `products` 배열이 비어있지 않다면, 배열에 포함된 모든 상품 ID를 참조하여 삭제합니다.
  3. `Product.deleteMany()`를 호출하여 해당 상품들을 삭제한 후, 결과를 콘솔에 출력합니다.

### 2. **index.js**: 농장 삭제 라우트
- 클라이언트로부터 `/farms/:id` DELETE 요청을 받으면 다음 작업이 실행됩니다:
  1. URL 파라미터 `:id`를 가져와 삭제할 농장을 식별합니다.
  2. `Farm.findByIdAndDelete(id)`를 호출하여 농장을 삭제합니다.
  3. 삭제 완료 후 `/farms` 페이지로 리다이렉트합니다.

- 이 과정에서 `findByIdAndDelete` 메서드는 내부적으로 `findOneAndDelete`를 호출하므로, **farm.js에 정의된 미들웨어가 자동으로 실행**됩니다.

## 실행 흐름
1. 클라이언트가 특정 농장을 삭제하기 위해 `/farms/:id` DELETE 요청을 보냅니다.
2. `Farm.findByIdAndDelete(id)`가 실행되어 데이터베이스에서 해당 농장이 삭제됩니다.
3. `farmSchema.post('findOneAndDelete')` 미들웨어가 트리거됩니다:
   - 삭제된 농장의 `products` 필드에 연결된 모든 상품 문서를 삭제합니다.
4. `/farms` 페이지로 리다이렉트됩니다.

## 미들웨어 활용 이점

### 1. 참조 무결성 유지
- 부모(농장)와 자식(상품) 관계에서 부모가 삭제될 때, 자식 데이터를 자동으로 삭제하여 데이터베이스의 참조 무결성을 유지할 수 있습니다.

### 2. 코드 간소화
- 삭제 로직을 라우트 코드에서 분리하여 가독성과 재사용성을 높입니다.

## 주의사항

### 1. 특정 메서드에서만 동작
- `post('findOneAndDelete')` 미들웨어는 `findOneAndDelete` 또는 `findByIdAndDelete` 메서드에서만 동작합니다. 
- `deleteOne`, `deleteMany`와 같은 다른 삭제 메서드에서는 동작하지 않으므로 주의가 필요합니다.

### 2. 성능 고려
- 관련된 문서가 많을 경우 삭제 작업이 느려질 수 있습니다. 비동기 작업 최적화 또는 작업 분리 방법을 고려해야 합니다.

## 결론
Mongoose의 미들웨어는 데이터베이스 작업을 자동화하고 참조 데이터를 효율적으로 관리하는 데 강력한 도구입니다. 이 예제는 단순하지만, 실무에서 데이터 정합성을 유지하고 복잡한 삭제 작업을 간소화하는 데 큰 도움을 줄 수 있습니다.
