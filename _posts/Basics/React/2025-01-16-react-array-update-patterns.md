---
title: "React.js 배열 업데이트 패턴 정리: 예제와 설명"
excerpt: "React 함수형 컴포넌트에서 상태로 관리되는 배열을 업데이트하는 일반적인 패턴을 예제 코드와 함께 정리하였습니다. 배열 추가, 제거, 수정 등 다양한 상황에 따른 코드를 제공합니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, 배열, 상태 관리]
permalink: /react/array-update-patterns/
toc: true
toc_sticky: true
date: 2025-01-16
last_modified_at: 2025-01-16
---

React.js에서 배열을 상태로 다룰 때는 **불변성**을 유지하는 것이 중요합니다. 이는 기존 배열을 직접 변경하는 대신, 새로운 배열을 생성하여 상태를 업데이트해야 함을 의미합니다. 아래에서는 함수형 컴포넌트 기준으로 배열을 업데이트하는 일반적인 패턴과 예제를 설명합니다.

## 1. 배열에 항목 추가하기

```javascript
const shoppingCart = [
  { id: 1, product: "HDMI Cable", price: 4 },
  { id: 2, product: "Easy Bake Oven", price: 28 },
  { id: 3, product: "Peach Pie", price: 6.5 },
];

const updatedCart = [...shoppingCart, { id: 4, product: "Coffee Mug", price: 7.99 }];
```

### 설명
- `...shoppingCart`는 기존 배열의 모든 항목을 복사합니다.
- 새로운 객체 `{ id: 4, product: "Coffee Mug", price: 7.99 }`를 복사된 배열 끝에 추가합니다.
- 원본 배열은 변경되지 않고, 새로운 배열만 생성됩니다.

### 사용 상황
- 장바구니에 새로운 제품을 추가하는 경우처럼 배열의 끝에 요소를 추가해야 할 때 사용됩니다.

---

## 2. 배열에서 특정 요소 제거하기

```javascript
const shoppingCart = [
  { id: 1, product: "HDMI Cable", price: 4 },
  { id: 2, product: "Easy Bake Oven", price: 28 },
  { id: 3, product: "Peach Pie", price: 6.5 },
];

const updatedCart = shoppingCart.filter((item) => item.id !== 2);
```

### 설명
- `filter`는 조건을 만족하는 요소만 유지하여 새 배열을 반환합니다.
- 여기서는 `item.id !== 2` 조건을 사용하여 `id`가 2인 항목을 제거합니다.
- 원본 배열은 변경되지 않습니다.

### 사용 상황
- 특정 조건을 충족하지 않는 요소를 배열에서 제거하고 싶을 때 유용합니다.
  - 예: 장바구니에서 특정 제품 삭제.

---

## 3. 배열의 모든 요소 업데이트하기

```javascript
const shoppingCart = [
  { id: 1, product: "HDMI Cable", price: 4 },
  { id: 2, product: "Easy Bake Oven", price: 28 },
  { id: 3, product: "Peach Pie", price: 6.5 },
];

const updatedCart = shoppingCart.map((item) => {
  return {
    ...item,
    product: item.product.toLowerCase(),
  };
});
```

### 설명
- `map`은 배열의 각 요소를 순회하며 새 값을 반환하여 새로운 배열을 생성합니다.
- 각 요소를 복사한 뒤(`...item`), `product` 값을 소문자로 변환하여 업데이트합니다.
- 원본 배열은 변하지 않습니다.

### 사용 상황
- 배열 내 모든 항목에 동일한 업데이트를 적용하고 싶을 때 사용됩니다.
  - 예: 모든 항목의 텍스트 형식 변경, 공통 속성 추가/수정.

---

## 4. 배열의 특정 요소 수정하기

```javascript
const shoppingCart = [
  { id: 1, product: "HDMI Cable", price: 4 },
  { id: 2, product: "Easy Bake Oven", price: 28 },
  { id: 3, product: "Peach Pie", price: 6.5 },
];

const updatedCart = shoppingCart.map((item) => {
  if (item.id === 3) {
    return { ...item, price: 10.99 };
  } else {
    return item;
  }
});
```

### 설명
- `map`을 사용하여 각 요소를 순회합니다.
- 특정 조건을 확인한 후(`if (item.id === 3)`), 해당 요소만 업데이트합니다.
- 업데이트가 필요한 경우 해당 요소를 복사한 뒤(`...item`), 원하는 속성을 수정합니다.

### 사용 상황
- 특정 조건에 맞는 항목만 수정해야 할 때 사용됩니다.
  - 예: 장바구니에서 특정 제품의 가격이나 수량 변경.

---

## 요약

| 작업                          | 메서드                | 주요 사용 상황                                               |
|-------------------------------|-----------------------|-------------------------------------------------------------|
| 배열에 요소 추가               | `[...arr, newItem]`  | 새로운 항목을 배열 끝에 추가                                 |
| 배열에서 요소 제거             | `filter`             | 특정 조건을 만족하지 않는 요소를 제거                       |
| 배열의 모든 요소 수정           | `map`                | 배열의 모든 항목에 공통된 변화를 적용                        |
| 배열의 특정 요소 수정           | `map` + 조건문       | 특정 조건에 맞는 항목만 업데이트                            |

React 상태 관리에서 이러한 패턴을 활용하면 **불변성**을 유지하며 안정적으로 상태를 업데이트할 수 있습니다.

