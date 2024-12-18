---
title: "Express + Mongoose CRUD 연습 예제: Farm Stand 제품 관리 애플리케이션"
excerpt: "Express, EJS, MongoDB를 활용한 간단한 농산물 제품 관리 웹 애플리케이션의 개요와 주요 기능을 설명합니다."
categories:
  - Node
tags:
  - [Express, EJS, CRUD, Backend]
permalink: /Node/express-mongoose-crude-practice1/
toc: true
toc_sticky: true
date: 2024-12-18
last_modified_at: 2024-12-18
---

## 프로젝트 개요

Farm Stand는 Express, EJS, MongoDB, Mongoose를 활용하여 구현된 간단한 제품 관리 애플리케이션입니다. 이 애플리케이션은 농산물 판매를 위한 제품을 생성, 조회, 수정, 삭제(CRUD)할 수 있는 웹 기반 시스템을 제공합니다.

---

## 주요 기술 스택

- **Express.js**: 백엔드 웹 프레임워크
- **EJS**: 서버 측 렌더링 템플릿 엔진
- **MongoDB**: NoSQL 데이터베이스
- **Mongoose**: MongoDB를 위한 ODM(Object Data Modeling) 라이브러리
- **Method-Override**: HTML 폼에서 PUT 및 DELETE 요청을 처리하기 위한 미들웨어
- **Node.js**: 서버 환경

---

## 프로젝트 디렉토리 구조

```
/views
 /products/
  new.ejs
  edit.ejs
  index.ejs
  show.ejs
/models
 product.js
 categories.js
index.js
```

### 주요 디렉토리 및 파일 설명
- **/views/products/**: EJS 템플릿 파일들이 위치한 디렉토리
  - **new.ejs**: 새로운 제품 생성 페이지
  - **edit.ejs**: 제품 수정 페이지
  - **index.ejs**: 전체 제품 목록 페이지
  - **show.ejs**: 단일 제품 상세 정보 페이지
- **/models/**: Mongoose 스키마 및 모델 정의 파일
  - **product.js**: 제품 관련 스키마 및 모델 정의
  - **categories.js**: 카테고리 데이터 정의
- **index.js**: 애플리케이션의 진입점 파일로 서버 설정 및 주요 라우트가 포함됨

---

## 주요 기능 및 라우트

### 1. 제품 목록 조회 (Index)
- **경로**: `/products`
- **HTTP 메서드**: GET
- **설명**: 모든 제품 또는 특정 카테고리의 제품을 조회하여 화면에 렌더링합니다.

### 2. 제품 생성 페이지
- **경로**: `/products/new`
- **HTTP 메서드**: GET
- **설명**: 새로운 제품을 생성할 수 있는 폼 페이지를 렌더링합니다.

### 3. 제품 생성 (Create)
- **경로**: `/products`
- **HTTP 메서드**: POST
- **설명**: 사용자가 입력한 데이터로 새로운 제품을 데이터베이스에 추가합니다.

### 4. 단일 제품 상세 조회
- **경로**: `/products/:id`
- **HTTP 메서드**: GET
- **설명**: 특정 제품의 상세 정보를 조회합니다.

### 5. 제품 수정 페이지
- **경로**: `/products/:id/edit`
- **HTTP 메서드**: GET
- **설명**: 선택한 제품을 수정할 수 있는 폼 페이지를 렌더링합니다.

### 6. 제품 수정 (Update)
- **경로**: `/products/:id`
- **HTTP 메서드**: PUT
- **설명**: 사용자가 수정한 데이터를 기반으로 기존 제품을 업데이트합니다.

### 7. 제품 삭제 (Delete)
- **경로**: `/products/:id`
- **HTTP 메서드**: DELETE
- **설명**: 선택한 제품을 데이터베이스에서 삭제합니다.

---

## 주요 코드 설명

### 1. 데이터베이스 연결
```javascript
mongoose.connect('mongodb://127.0.0.1:27017/farmStand')
    .then(() => {
        console.log("MONGO CONNECTION OPEN");
    })
    .catch(err => {
        console.log('OH NO MONGO CONNECTION ERROR!');
        console.log(err);
    });
```
MongoDB에 연결하고 성공 또는 실패 메시지를 출력합니다.

### 2. 제품 스키마 및 모델 정의
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
        enum: ['Fruit', 'Vegetable', 'Dairy']
    }
});

const Product = mongoose.model('Product', productSchema);
module.exports = Product;
```
- 제품 이름, 가격, 카테고리를 포함한 Mongoose 스키마를 정의합니다.
- 카테고리는 "Fruit", "Vegetable", "Dairy" 값만 허용합니다.

### 3. 제품 목록 조회
```javascript
app.get('/products', async (req, res) => {
    const { category } = req.query;
    if (category) {
        const products = await Product.find({ category });
        res.render('products/index', { products, category, categories });
    } else {
        const products = await Product.find({});
        res.render('products/index', { products, category: 'All', categories });
    }
});
```
- URL 쿼리 파라미터 `category`를 기반으로 필터링된 제품을 조회하거나 모든 제품을 조회합니다.

### 4. 제품 생성
```javascript
app.post('/products', async (req, res) => {
    const newProduct = new Product(req.body);
    await newProduct.save();
    res.redirect(`/products/${newProduct._id}`);
});
```
- 클라이언트로부터 받은 데이터를 기반으로 새로운 제품을 생성하고 상세 페이지로 리디렉션합니다.

---

## 화면 구성

### 1. 제품 목록 페이지 (index.ejs)
```html
<h1>
    <%= category.charAt(0).toUpperCase() + category.slice(1) %> Products
</h1>
<ul>
    <% for(let product of products) { %>
        <li>
            <a href="/products/<%= product._id %>">
                <%= product.name %>
            </a>
        </li>
    <% } %>
</ul>
<a href="/products/new">Add new product</a>
```
- 현재 카테고리의 모든 제품을 목록 형태로 보여줍니다.

---

## 프로젝트 실행 방법

1. MongoDB 실행
2. 필요한 패키지 설치
   ```bash
   npm install
   ```
3. 서버 실행
   ```bash
   node index.js
   ```
4. 브라우저에서 접속
   ```
   http://localhost:3000/products
   ```

---

## 개선 사항 및 추후 고려할 내용

1. **사용자 인증**: 
   - 사용자 인증 및 권한 관리를 추가하여 특정 사용자만 제품을 추가, 수정, 삭제할 수 있도록 개선합니다.

2. **카테고리 관리 기능**: 
   - 카테고리 추가/수정/삭제 기능을 제공하여 더 유연한 데이터 관리를 지원합니다.

3. **프론트엔드 개선**: 
   - Bootstrap 또는 TailwindCSS와 같은 UI 라이브러리를 적용하여 더 나은 사용자 경험을 제공합니다.

4. **에러 처리**: 
   - 모든 라우트에 대한 에러 처리 미들웨어를 추가하여 사용자에게 적절한 에러 메시지를 표시합니다.

5. **RESTful API 확장**: 
   - JSON 형식의 RESTful API를 추가로 제공하여 외부 애플리케이션에서도 데이터를 연동할 수 있도록 확장합니다.

6. **배포**: 
   - Heroku, Vercel 또는 AWS와 같은 클라우드 플랫폼에 배포하여 실제 운영 환경에서 테스트합니다.

7. **테스트**:
   - Mocha, Chai 또는 Jest를 사용하여 유닛 테스트 및 통합 테스트를 추가합니다.

