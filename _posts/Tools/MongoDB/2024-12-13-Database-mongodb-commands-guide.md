---
title: "MongoDB의 기본 명령어와 주요 예제 가이드"
excerpt: "MongoDB의 데이터베이스, 컬렉션 관리 및 문서 조작 명령어를 예제와 함께 알아보고, NoSQL 데이터베이스의 활용법을 이해합니다."
categories:
  - MongoDB
tags:
  - [MongoDB, NoSQL, Database, 데이터베이스]
permalink: /mongodb/mongodb-commands-guide/
toc: true
toc_sticky: true
date: 2024-12-13
last_modified_at: 2024-12-13
---

MongoDB는 NoSQL 데이터베이스로, 데이터를 JSON 유사 형식으로 저장하고 관리합니다. 기본 명령어는 데이터베이스 관리, 컬렉션 관리, 문서 조작 등을 수행하는 데 사용됩니다. 주요 명령어를 카테고리별로 나누어 예시와 함께 설명하겠습니다.

---

## 1. 데이터베이스 관리 명령어

### 데이터베이스 보기

```bash
show dbs
```

현재 MongoDB에 있는 모든 데이터베이스를 표시합니다.

### 데이터베이스 선택/생성

```javascript
use myDatabase
```

`myDatabase`라는 데이터베이스를 선택합니다. 데이터베이스가 존재하지 않으면 자동으로 생성됩니다.

### 현재 사용 중인 데이터베이스 확인

```javascript
db
```

현재 작업 중인 데이터베이스의 이름을 표시합니다.

---

## 2. 컬렉션 관리 명령어

### 컬렉션 보기

```javascript
show collections
```

현재 데이터베이스에 있는 모든 컬렉션(테이블 유사체)을 표시합니다.

### 컬렉션 생성

```javascript
db.createCollection("myCollection")
```

`myCollection`이라는 이름의 새로운 컬렉션을 생성합니다.

### 컬렉션 삭제

```javascript
db.myCollection.drop()
```

`myCollection` 컬렉션을 삭제합니다.

---

## 3. 문서 조작 명령어

### 문서 삽입

```javascript
db.myCollection.insertOne({ name: "Alice", age: 25, city: "Seoul" })
db.myCollection.insertMany([
  { name: "Bob", age: 30, city: "Busan" },
  { name: "Charlie", age: 35, city: "Incheon" }
])
```

- `insertOne`: 한 개의 문서를 삽입합니다.
- `insertMany`: 여러 개의 문서를 삽입합니다.

### 문서 조회

```javascript
db.myCollection.find()
db.myCollection.find({ city: "Seoul" })
db.myCollection.find({ age: { $gt: 28 } })  // age > 28
```

- `find()`: 모든 문서를 조회합니다.
- 조건을 넣으면 특정 조건에 맞는 문서를 조회할 수 있습니다.

### 문서 수정

```javascript
db.myCollection.updateOne(
  { name: "Alice" },
  { $set: { age: 26 } }
)
db.myCollection.updateMany(
  { city: "Busan" },
  { $set: { country: "South Korea" } }
)
```

- `updateOne`: 첫 번째 일치 문서를 수정합니다.
- `updateMany`: 모든 일치 문서를 수정합니다.
- `$set`: 필드를 업데이트합니다.

### 문서 삭제

```javascript
db.myCollection.deleteOne({ name: "Alice" })
db.myCollection.deleteMany({ city: "Busan" })
```

- `deleteOne`: 첫 번째 일치 문서를 삭제합니다.
- `deleteMany`: 모든 일치 문서를 삭제합니다.

---

## 4. 기타 명령어

### 문서 개수 확인

```javascript
db.myCollection.countDocuments()
```

컬렉션에 있는 문서의 개수를 반환합니다.

### 정렬 및 제한

```javascript
db.myCollection.find().sort({ age: 1 }).limit(2)
```

- `sort`: 정렬 기준 지정 (`1`: 오름차순, `-1`: 내림차순).
- `limit`: 반환할 문서의 개수를 제한합니다.

### 인덱스 생성

```javascript
db.myCollection.createIndex({ name: 1 })
```

`name` 필드에 대해 오름차순 인덱스를 생성합니다.

### 데이터베이스 삭제

```javascript
db.dropDatabase()
```

현재 데이터베이스를 삭제합니다.

---

## MongoDB Aggregation (데이터 분석용)

### 기본 Aggregation 예제

```javascript
db.myCollection.aggregate([
  { $match: { city: "Seoul" } }, // 조건
  { $group: { _id: "$city", avgAge: { $avg: "$age" } } } // 그룹화 및 평균 계산
])
```

- `$match`: 조건 필터링.
- `$group`: 데이터를 그룹화.
- `$avg`: 평균 계산.

---

위 명령어들은 MongoDB의 핵심 기능을 다룹니다. 추가적으로 더 복잡한 쿼리가 필요하거나 특정 상황에서 사용할 수 있는 명령어에 대해 더 알고 싶다면 댓글로 질문해주세요! 😊

