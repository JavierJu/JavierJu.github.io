---
title: "Node.js 모델 코드 분리: (예: categories)"
excerpt: "Node.js에서 categories 데이터를 효과적으로 관리하기 위해 별도 모듈로 분리하거나 모델에 포함하는 두 가지 리팩터링 방법을 소개합니다."
categories:
  - Node
  
tags:
  - [JavaScript, Node.js, Backend, 코드 리팩터링]
permalink: /Node/nodejs-categories-refactor/
toc: true
toc_sticky: true
date: 2024-12-16
last_modified_at: 2024-12-16
---

Node.js 애플리케이션에서 데이터를 어떻게 관리하느냐는 코드의 유지보수성과 확장성에 중요한 영향을 미칩니다. 이 글에서는 특정 데이터(예: `categories`)를 효과적으로 관리하기 위해 사용하는 두 가지 리팩터링 방법을 살펴보겠습니다.

---

## 기존 코드 구조
다음은 리팩터링 대상 코드입니다.
`index.js`에서 `categories`를 정의하고, 이를 뷰 템플릿에 전달하고 있습니다.

### index.js
```javascript
const Product = require('./models/product');

const categories = ['fruit', 'vegetable', 'diary'];

// Page for creating a new product
app.get('/products/new', (req, res) => {
    res.render('products/new', { categories });
});
```

그리고 `product.js`는 `categories`를 포함한 데이터베이스 모델을 정의하고 있습니다.

### product.js
```javascript
const mongoose = require('mongoose');

const productSchema = new mongoose.Schema({
    name: {
        type: String,
        required: true
    },
    price: {
        type: Number,
        required: true,
        min: 0
    },
    category: {
        type: String,
        enum: ['fruit', 'vegetable', 'diary']
    }
});

const Product = mongoose.model('Product', productSchema);

module.exports = Product;
```

현재 구조에서는 `categories`가 `index.js`와 `product.js` 양쪽에서 필요합니다. 이를 개선하려면 데이터를 한 곳에서 관리하는 리팩터링이 필요합니다.

---

## 리팩터링 방법

### 방법 1: categories를 별도의 모듈로 분리
`categories`를 별도의 파일로 분리하면, 중복 정의를 제거하고 데이터 관리가 더 용이해집니다.

#### 새 파일: `categories.js`
```javascript
const categories = ['fruit', 'vegetable', 'diary'];
module.exports = categories;
```

#### product.js
```javascript
const mongoose = require('mongoose');
const categories = require('../categories');

const productSchema = new mongoose.Schema({
    name: {
        type: String,
        required: true
    },
    price: {
        type: Number,
        required: true,
        min: 0
    },
    category: {
        type: String,
        enum: categories
    }
});

const Product = mongoose.model('Product', productSchema);

module.exports = Product;
```

#### index.js
```javascript
const categories = require('./categories');
const Product = require('./models/product');

// Page for creating a new product
app.get('/products/new', (req, res) => {
    res.render('products/new', { categories });
});
```

> **장점**: `categories`가 별도의 파일로 관리되므로 코드의 역할 분리가 명확해집니다. 또한 `categories`를 다른 파일에서도 재사용할 수 있습니다.

---

### 방법 2: product.js에 categories 포함
간단한 애플리케이션에서는 `categories`를 모델 파일 내부에 포함하는 것도 하나의 방법입니다.

#### product.js
```javascript
const mongoose = require('mongoose');

const categories = ['fruit', 'vegetable', 'diary'];

const productSchema = new mongoose.Schema({
    name: {
        type: String,
        required: true
    },
    price: {
        type: Number,
        required: true,
        min: 0
    },
    category: {
        type: String,
        enum: categories
    }
});

const Product = mongoose.model('Product', productSchema);

module.exports = { Product, categories };
```

#### index.js
```javascript
const { Product, categories } = require('./models/product');

// Page for creating a new product
app.get('/products/new', (req, res) => {
    res.render('products/new', { categories });
});
```

> **장점**: 모델 파일 내에서 데이터베이스 관련 정보를 한곳에 집중시킬 수 있습니다.

> **단점**: 뷰와 모델의 역할이 혼합될 가능성이 있습니다.

---

## 결론
- **방법 1**은 코드의 역할을 분리하고 재사용성을 높일 수 있어 추천됩니다.
- **방법 2**는 애플리케이션이 간단하거나, 모델과 관련된 데이터가 많지 않을 경우 적합합니다.

코드의 복잡성, 프로젝트의 규모, 그리고 팀의 협업 방식을 고려하여 적합한 리팩터링 방법을 선택하세요!

