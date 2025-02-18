---
title: "IndexedDB 완벽 가이드: 개념부터 실전 활용까지"
excerpt: "IndexedDB의 개념, 특징, 작동 원리, 기본 사용법, 장단점 및 실전 활용법을 코드 예제와 함께 자세히 설명합니다."
categories:
  - Web
  - Database
  - Browser API

tags:
  - [IndexedDB, JavaScript, Database, Web Storage, Offline]
permalink: /web/indexeddb-guide/
toc: true
toc_sticky: true
date: 2025-02-16
last_modified_at: 2025-02-16
---

## 1. IndexedDB란?

**IndexedDB**는 브라우저에 내장된 **비관계형(NoSQL) 데이터베이스**로, 웹 애플리케이션이 로컬에서 대용량 데이터를 저장하고 검색할 수 있도록 해주는 API입니다.

기존의 `localStorage`와 `sessionStorage`와 같은 웹 스토리지보다 훨씬 더 강력한 기능을 제공하며, 데이터가 객체 형태로 저장되므로 관계형 데이터베이스(RDBMS)처럼 정형화된 스키마 없이 유연한 데이터 관리를 할 수 있습니다.

## 2. IndexedDB의 주요 특징

### ✅ 비관계형(NoSQL) 데이터베이스

- 테이블이 아닌 **오브젝트 스토어(Object Store)**를 사용하여 데이터를 저장합니다.
- JSON 형식의 객체를 그대로 저장할 수 있습니다.

### ✅ 대용량 데이터 저장 가능

- `localStorage`의 최대 용량(약 5MB)을 초과하여 **수백 MB에서 수 GB까지** 저장 가능합니다.
- 브라우저마다 제한이 다를 수 있지만, 일반적으로 50MB 이상을 지원합니다.

### ✅ 비동기 API

- **Promise 기반 API**를 제공하여 UI가 멈추지 않고 데이터 작업을 수행할 수 있습니다.
- `localStorage`와 달리 동기적으로 데이터를 처리하지 않기 때문에 성능이 뛰어납니다.

### ✅ 트랜잭션 지원

- 데이터의 무결성을 유지하기 위해 **트랜잭션(Transaction)**을 지원합니다.
- 여러 작업을 하나의 트랜잭션으로 묶어 원자적으로 실행할 수 있습니다.

### ✅ 인덱싱(Indexing) 기능 제공

- 특정 속성을 기준으로 빠른 검색이 가능하도록 **인덱스(Index)**를 생성할 수 있습니다.
- 대량의 데이터를 검색할 때 성능을 최적화할 수 있습니다.

### ✅ 브라우저 내장 기능

- 별도의 플러그인 없이 **모든 최신 브라우저에서 사용 가능**합니다.
- Chrome, Firefox, Edge, Safari 등의 최신 브라우저에서 지원됩니다.

---

## 3. IndexedDB의 작동 원리

IndexedDB는 일반적인 RDBMS와 다르게 **오브젝트 스토어(Object Store)**를 사용하여 데이터를 저장합니다. 기본적인 작동 원리는 다음과 같습니다.

### 📌 1) 데이터베이스 생성 및 버전 관리

IndexedDB 데이터베이스는 **버전(version)**을 가집니다. 데이터베이스를 처음 생성할 때 버전을 지정하며, 새로운 버전으로 업그레이드할 때 `onupgradeneeded` 이벤트가 발생하여 스키마 변경을 수행할 수 있습니다.

```javascript
let request = indexedDB.open("myDatabase", 1);

request.onupgradeneeded = function (event) {
  let db = event.target.result;
  let objectStore = db.createObjectStore("customers", {
    keyPath: "id",
    autoIncrement: true,
  });
  objectStore.createIndex("name", "name", { unique: false });
};

request.onsuccess = function (event) {
  let db = event.target.result;
  console.log("IndexedDB가 성공적으로 열렸습니다.");
};

request.onerror = function (event) {
  console.error("데이터베이스 오류:", event.target.errorCode);
};
```

### 📌 2) 데이터 추가 (Add)

```javascript
let transaction = db.transaction(["customers"], "readwrite");
let objectStore = transaction.objectStore("customers");
let customer = { name: "홍길동", email: "hong@example.com" };
let addRequest = objectStore.add(customer);

addRequest.onsuccess = function () {
  console.log("고객 정보가 추가되었습니다.");
};
```

### 📌 3) 데이터 조회 (Read)

```javascript
let transaction = db.transaction(["customers"], "readonly");
let objectStore = transaction.objectStore("customers");
let getRequest = objectStore.get(1); // id=1인 데이터 조회

getRequest.onsuccess = function (event) {
  if (event.target.result) {
    console.log("조회된 데이터:", event.target.result);
  } else {
    console.log("데이터가 존재하지 않습니다.");
  }
};
```

### 📌 4) 데이터 업데이트 (Update)

```javascript
let transaction = db.transaction(["customers"], "readwrite");
let objectStore = transaction.objectStore("customers");
let updateRequest = objectStore.get(1);

updateRequest.onsuccess = function (event) {
  let data = event.target.result;
  data.email = "new_email@example.com";
  objectStore.put(data);
};
```

### 📌 5) 데이터 삭제 (Delete)

```javascript
let transaction = db.transaction(["customers"], "readwrite");
let objectStore = transaction.objectStore("customers");
let deleteRequest = objectStore.delete(1);

deleteRequest.onsuccess = function () {
  console.log("데이터 삭제 완료");
};
```

---

## 4. IndexedDB의 장단점

### ✅ 장점

- **대용량 데이터 저장 가능** (MB ~ GB 단위 저장 가능)
- **비동기 처리**로 UI 성능 저하 없음
- **인덱스를 통한 빠른 검색 가능**
- **트랜잭션 지원**으로 데이터 무결성 보장
- **브라우저 내장 기능**으로 추가적인 플러그인 필요 없음

### ❌ 단점

- **API가 복잡함** (기본적인 CRUD 연산도 코드가 길어질 수 있음)
- **브라우저 간 제한이 다를 수 있음** (스토리지 크기 등)
- **동기적인 IndexedDB API 없음** (Promise나 이벤트 기반으로만 동작)

---

## 5. IndexedDB 활용 사례

- **오프라인 웹 애플리케이션** (네트워크 연결 없이 데이터 저장/조회 가능)
- **대용량 캐싱 시스템** (서버 요청을 최소화하여 성능 향상)
- **브라우저 내 데이터 저장** (사용자 데이터, 설정 정보 등 저장)
- **이미지/파일 저장** (Blob 데이터 저장 가능)

IndexedDB는 강력한 기능을 제공하는 만큼 초기 학습이 필요하지만, 한 번 익히면 매우 강력한 웹 애플리케이션 개발 도구가 될 수 있습니다.
