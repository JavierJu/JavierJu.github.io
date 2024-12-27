---
title: "MongoDB의 관계에 대한 자세한 이해: One-to-Many, Many-to-Many 등"
excerpt: "MongoDB에서 One-to-One, One-to-Many, Many-to-Many 관계를 다루는 방법과 구현 사례를 통해 문서 지향 데이터베이스의 유연성을 탐구합니다."
categories:
  - MongoDB
tags:
  - [NoSQL, MongoDB, 데이터베이스, 관계형 데이터]
permalink: /mongodb/mongodb-relationships/
toc: true
toc_sticky: true
date: 2024-12-25
last_modified_at: 2024-12-25
---

MongoDB는 관계형 데이터베이스(RDBMS)와 달리 **스키마가 없는 NoSQL 데이터베이스**로, 데이터를 JSON 형태로 저장하는 문서 지향 데이터베이스입니다. 관계형 데이터베이스에서 흔히 다루는 관계(One-to-Many, Many-to-Many 등)도 MongoDB에서 구현할 수 있습니다. 그러나 MongoDB는 관계형 데이터베이스와 달리 데이터를 중첩하거나 연결하는 방식으로 이러한 관계를 관리합니다.

다음은 MongoDB에서 **One-to-One**, **One-to-Many**, **Many-to-Many** 관계를 표현하는 방법과 사례입니다.

---

## 1. **One-to-One 관계**

- **설명**: 하나의 문서가 다른 하나의 문서와 연결된 관계입니다.
- **예제**: 사용자와 프로필 정보
- **구현 방법**:
  - **중첩 문서(Nested Document)**:
    한 문서에 다른 문서를 포함시켜 저장합니다.
    ```json
    {
      "_id": 1,
      "name": "John Doe",
      "profile": {
        "age": 30,
        "gender": "Male",
        "address": "123 Main St"
      }
    }
    ```
  - **참조(Reference)**:
    별도의 컬렉션에서 ObjectId로 연결합니다.
    ```json
    // users 컬렉션
    {
      "_id": 1,
      "name": "John Doe",
      "profile_id": ObjectId("64abc123def4567890")
    }
    // profiles 컬렉션
    {
      "_id": ObjectId("64abc123def4567890"),
      "age": 30,
      "gender": "Male",
      "address": "123 Main St"
    }
    ```

---

## 2. **One-to-Many 관계**

- **설명**: 하나의 문서가 여러 문서와 연결된 관계입니다.
- **예제**: 사용자와 주문 정보
- **구현 방법**:
  - **중첩 문서(Nested Document)**:
    한 문서 내에 여러 문서를 배열로 저장합니다.
    ```json
    {
      "_id": 1,
      "name": "John Doe",
      "orders": [
        { "order_id": 101, "amount": 50 },
        { "order_id": 102, "amount": 75 }
      ]
    }
    ```
  - **참조(Reference)**:
    주문 문서를 별도의 컬렉션에 저장하고, 사용자 ID로 연결합니다.
    ```json
    // users 컬렉션
    {
      "_id": 1,
      "name": "John Doe"
    }
    // orders 컬렉션
    {
      "_id": 101,
      "user_id": 1,
      "amount": 50
    }
    {
      "_id": 102,
      "user_id": 1,
      "amount": 75
    }
    ```

---

## 3. **Many-to-Many 관계**

- **설명**: 여러 문서가 여러 문서와 연결된 관계입니다.
- **예제**: 학생과 수업 정보
- **구현 방법**:
  - **중첩 배열로 구현**:
    한 컬렉션에서 배열에 다른 문서의 ID를 저장합니다.
    ```json
    // students 컬렉션
    {
      "_id": 1,
      "name": "Alice",
      "classes": [1001, 1002]
    }
    {
      "_id": 2,
      "name": "Bob",
      "classes": [1002, 1003]
    }
    // classes 컬렉션
    {
      "_id": 1001,
      "name": "Math"
    }
    {
      "_id": 1002,
      "name": "Science"
    }
    {
      "_id": 1003,
      "name": "History"
    }
    ```
  - **교차 테이블(Collection)**:
    관계를 명시적으로 관리하는 컬렉션을 사용합니다.
    ```json
    // student_classes 컬렉션
    {
      "student_id": 1,
      "class_id": 1001
    }
    {
      "student_id": 1,
      "class_id": 1002
    }
    {
      "student_id": 2,
      "class_id": 1002
    }
    {
      "student_id": 2,
      "class_id": 1003
    }
    ```

---

## MongoDB에서 관계를 선택할 때 고려할 점

1. **데이터 중첩(Nested)**:
   - 데이터를 함께 자주 조회할 경우 적합합니다.
   - 하지만 데이터 크기가 커지면 비효율적일 수 있습니다.
2. **참조(References)**:
   - 데이터가 자주 변경되거나 독립적으로 관리되어야 할 때 적합합니다.
   - 참조를 사용할 경우 `lookup`으로 조인하는 비용이 발생합니다.
3. **데이터 크기와 복잡성**:
   - 중첩 문서가 너무 크면 제한(16MB)을 초과할 수 있습니다.
4. **읽기와 쓰기 성능**:
   - 읽기 성능을 위해 중첩 문서를, 쓰기 성능과 유연성을 위해 참조를 사용하는 것이 일반적입니다.

MongoDB에서는 관계를 설정할 때 설계의 유연성과 성능을 고려하여 중첩과 참조 중 적합한 방식을 선택해야 합니다.

