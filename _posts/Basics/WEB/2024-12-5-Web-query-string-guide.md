---
title: "WebAPI에서 Query String의 역할과 활용법"
excerpt: "WebAPI의 Query String 구조, 사용 사례, 그리고 실전 활용법을 알아봅니다."
categories:
  - Web
tags:
  - [WebAPI, Query String, JavaScript, PHP, REST API]
permalink: /Web/query-string-guide/
toc: true
toc_sticky: true
date: 2024-12-5
last_modified_at: 2024-12-5
---

## Query String이란?

**Query String**은 HTTP 요청의 URL에 데이터를 전달하기 위한 주요 방법 중 하나입니다. 클라이언트가 서버로 정보를 보낼 때, URL 끝에 `?`를 붙여 `key=value` 형태로 데이터를 추가하며, 여러 데이터를 보낼 경우 `&`로 구분합니다.

## Query String 구조

https://example.com/resource?key1=value1&key2=value2


- **Base URL**: `https://example.com/resource`
- **Query String 시작**: `?`
- **Key-Value Pair**: `key1=value1`
- **Separator**: `&` (여러 쌍을 구분)

---

## Query String의 주요 특징

1. **Key-Value 쌍**  
   각 데이터는 고유한 `key`와 `value`로 구성됩니다. 특수문자와 공백은 URL 인코딩이 필요합니다.  
   (예: 공백 → `%20`, 특수문자 → `%` 인코딩)

2. **GET 요청과 주로 사용**  
   URL에 포함되므로 브라우저에서 쉽게 확인 가능하며, 데이터를 서버로 보낼 때 유용합니다.  
   단, 데이터가 노출되므로 민감한 정보에는 부적합합니다.

3. **길이 제한**  
   URL 길이는 브라우저와 서버에 따라 제한이 있으므로 지나치게 긴 데이터를 보낼 수 없습니다.

4. **RESTful 설계**  
   필터링, 검색, 정렬, 페이지네이션 등의 작업에 사용됩니다.

---

## Query String 사용 예시

### 1. 필터링
```plaintext
GET /products?category=electronics&brand=sony
```
- `category=electronics`: 전자 제품만 필터링
- `brand=sony`: Sony 브랜드만 검색

### 2. 페이지네이션
```plaintext
GET /articles?page=2&limit=10
```
- `page=2`: 두 번째 페이지
- `limit=10`: 한 페이지에 10개의 항목 표시

### 3. 검색
```plaintext
GET /search?query=smartphone
```
- `query=smartphone`: "smartphone" 키워드 검색

---

## 서버에서 Query String 처리하기

### PHP 코드 예시
```php
<?php
// Query String에서 key-value를 가져옴
$category = $_GET['category']; // 'electronics'
$brand = $_GET['brand'];       // 'sony'

echo "Category: $category, Brand: $brand";
?>
```

### 자바스크립트 코드 예시
`URLSearchParams`를 사용하여 Query String을 처리할 수 있습니다.

```javascript
const url = new URL('https://example.com/products?category=electronics&brand=sony');
const params = new URLSearchParams(url.search);

// Query String 값 읽기
console.log(params.get('category')); // 'electronics'
console.log(params.get('brand'));    // 'sony'

// 새로운 key-value 추가
params.append('sort', 'price_asc');
console.log(params.toString()); // 'category=electronics&brand=sony&sort=price_asc'
```

---

## Query String 활용을 위한 Best Practices

1. **명확한 키 이름**  
   - `q`, `sort`, `page` 등 직관적이고 의미 있는 이름 사용.

2. **짧은 데이터 전달**  
   - URL 길이 제한을 고려하여 짧은 데이터를 전달합니다.

3. **보안 고려**  
   - 민감한 정보는 Query String 대신 POST 요청의 Body를 사용합니다.

4. **RESTful 디자인 준수**  
   - Query String은 리소스를 조회하거나 필터링하는 데 사용하고, 상태 변경에는 사용하지 않습니다.

---

Query String은 간단하면서도 강력한 데이터 전달 방식입니다. 적절히 사용하여 더 나은 웹 서비스를 설계해 보세요!
