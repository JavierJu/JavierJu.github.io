---
title: "IndexedDB vs MongoDB: 차이점과 활용 방법"
excerpt: "IndexedDB와 MongoDB의 개념, 차이점, 데이터 저장 방식, 주요 기능 비교, 활용 사례를 코드 예제와 함께 자세히 설명합니다."
categories:
  - Web
  - JavaScript
  - Database
  - NoSQL
  - MongoDB

tags:
  - [IndexedDB, MongoDB, JavaScript, Database, Web Storage, NoSQL]
permalink: /web/indexeddb-vs-mongodb/
toc: true
toc_sticky: true
date: 2025-02-16
last_modified_at: 2025-02-16
---

## 1. IndexedDB vs MongoDB 개념 비교

|                        | **IndexedDB**                                              | **MongoDB**                                                       |
| ---------------------- | ---------------------------------------------------------- | ----------------------------------------------------------------- |
| **종류**               | 브라우저 내장 NoSQL 데이터베이스                           | 서버 기반 NoSQL 데이터베이스                                      |
| **주 용도**            | 웹 애플리케이션의 로컬 데이터 저장 (오프라인 지원, 캐싱)   | 서버에서 데이터를 저장하고 여러 클라이언트가 공유                 |
| **데이터 저장 위치**   | 클라이언트(웹 브라우저) 내부                               | 서버(클라우드, 온프레미스)                                        |
| **동작 방식**          | 클라이언트 측에서 동작하며, API를 통해 데이터 저장 및 관리 | 서버 측에서 동작하며, 클라이언트(웹, 앱)에서 API 요청을 통해 접근 |
| **네트워크 필요 여부** | **불필요** (로컬에서 동작)                                 | **필요** (서버와의 연결 필수)                                     |
| **데이터 구조**        | JSON 형식 객체 저장 (Object Store)                         | BSON 형식의 Document 저장 (Collection)                            |
| **확장성**             | 제한적 (클라이언트 저장 공간 한정)                         | **수평적 확장 가능** (Sharding, Replication 지원)                 |
| **보안**               | 브라우저 내 스토리지 사용 (보안 취약)                      | 강력한 인증 및 보안 시스템 제공                                   |

## 2. 데이터 저장 방식 비교

### **📌 IndexedDB (Object Store)**

```json
{
  "id": 1,
  "name": "홍길동",
  "email": "hong@example.com"
}
```

- 데이터를 Key-Value 형태로 저장.
- 특정 속성을 인덱스로 설정하여 빠르게 검색 가능.
- `localStorage`보다 복잡한 데이터 구조를 저장 가능.

### **📌 MongoDB (Collection - Document)**

```json
{
  "_id": ObjectId("603d5a8c8f1a4b6d5a4c1234"),
  "name": "홍길동",
  "email": "hong@example.com",
  "orders": [
    {
      "product": "노트북",
      "price": 1200000
    },
    {
      "product": "마우스",
      "price": 30000
    }
  ]
}
```

- `ObjectId`를 기본 키로 자동 생성.
- JSON과 유사한 **BSON(Binary JSON)** 포맷을 사용.
- 컬렉션 간 관계형 데이터도 저장 가능.

## 3. 주요 기능 비교

| 기능                   | **IndexedDB**                      | **MongoDB**                                  |
| ---------------------- | ---------------------------------- | -------------------------------------------- |
| **트랜잭션 지원**      | O (트랜잭션 기능 제공)             | O (ACID 트랜잭션 지원)                       |
| **인덱싱 (Indexing)**  | O (속성 기반 인덱스 지원)          | O (고급 인덱싱, 복합 인덱스 지원)            |
| **검색 성능**          | 기본적인 속성 검색 지원            | 고급 검색 기능 제공 (정규 표현식, 복합 쿼리) |
| **복잡한 데이터 관계** | 관계형 구조를 직접 표현하기 어려움 | 컬렉션 간 관계형 데이터 표현 가능            |
| **확장성**             | 클라이언트 장치 저장공간 제한      | **수평 확장 가능 (Sharding 지원)**           |
| **보안**               | 브라우저 내장 (취약)               | **인증, 접근 제어, 암호화 기능 제공**        |

## 4. IndexedDB와 MongoDB를 함께 사용하는 방법

### **✅ IndexedDB를 캐싱 용도로 활용**

```javascript
let request = indexedDB.open("myDatabase", 1);
request.onsuccess = function (event) {
  let db = event.target.result;
  let transaction = db.transaction(["customers"], "readwrite");
  let store = transaction.objectStore("customers");
  store.add({ id: 1, name: "홍길동", email: "hong@example.com" });
};
```

### **✅ IndexedDB 데이터를 MongoDB로 동기화**

```javascript
fetch("https://api.example.com/customers", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ id: 1, name: "홍길동", email: "hong@example.com" }),
})
  .then((response) => response.json())
  .then((data) => console.log("MongoDB에 동기화 완료", data));
```

## 5. 언제 IndexedDB vs MongoDB를 선택할까?

| 사용 사례                       | IndexedDB 선택 | MongoDB 선택 |
| ------------------------------- | -------------- | ------------ |
| **오프라인 지원 필요**          | ✅             | ❌           |
| **대규모 사용자 데이터 관리**   | ❌             | ✅           |
| **캐싱 기능 필요**              | ✅             | ❌           |
| **여러 사용자 간 데이터 공유**  | ❌             | ✅           |
| **복잡한 관계형 데이터 저장**   | ❌             | ✅           |
| **빠른 클라이언트 데이터 접근** | ✅             | ❌           |
| **보안이 중요한 데이터**        | ❌             | ✅           |

💡 **결론: IndexedDB는 클라이언트 측 로컬 데이터 저장용, MongoDB는 서버 측 데이터 관리용으로 사용되며, 함께 활용하면 강력한 애플리케이션을 만들 수 있습니다.** 🚀
