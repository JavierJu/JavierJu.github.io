---
title: "MongoDB 스키마 설계 원칙과 예제 코드"
excerpt: "MongoDB에서 One-to-N 관계를 설계하는 방법과 비정규화 및 참조 설계의 주요 원칙을 예제 코드와 함께 정리했습니다."
categories:
  - Tools
  
tags:
  - [MongoDB, Database, Backend, 스키마 설계]
permalink: /Tools/mongodb-schema-design/
toc: true
toc_sticky: true
date: 2024-12-25
last_modified_at: 2024-12-25
---

## MongoDB 스키마 설계 원칙 요약과 예제

### 기본 개념
MongoDB에서 One-to-N 관계를 설계할 때 다음을 고려해야 합니다:
1. **N 측 데이터가 독립적으로 접근될 필요가 있는가?**
2. 관계의 **카디널리티**: "one-to-few", "one-to-many", "one-to-squillions" 중 무엇인가?

#### **One-to-few**: N 측을 임베드
- 카디널리티가 적고, N 측 데이터가 독립적으로 사용되지 않을 때 적합.

```javascript
db.person.findOne()
{
  name: 'Kate Monster',
  ssn: '123-456-7890',
  addresses : [
    { street: '123 Sesame St', city: 'Anytown', cc: 'USA' },
    { street: '123 Avenue Q', city: 'New York', cc: 'USA' }
  ]
}
```

#### **One-to-many**: 참조 배열 사용
- 카디널리티가 많거나, N 측 데이터가 독립적으로 사용될 필요가 있을 때 사용.

```javascript
db.parts.findOne()
{
  _id : ObjectID('AAAA'),
  partno : '123-aff-456',
  name : '#4 grommet',
  qty: 94,
  cost: 0.94,
  price: 3.99
}

db.products.findOne()
{
  name : 'left-handed smoke shifter',
  manufacturer : 'Acme Corp',
  catalog_number: 1234,
  parts : [
    ObjectID('AAAA'),  // #4 grommet 참조
    ObjectID('F17C'),  // 다른 부품 참조
    ObjectID('D2AA')
  ]
}

// 관련 Parts를 가져오는 쿼리
product = db.products.findOne({catalog_number: 1234});
product_parts = db.parts.find({_id: { $in : product.parts }}).toArray();
```

#### **One-to-squillions**: 부모 참조 사용
- 매우 많은 N 측 데이터가 있을 때 부모를 참조하여 설계.

```javascript
db.hosts.findOne()
{
  _id : ObjectID('AAAB'),
  name : 'goofy.example.com',
  ipaddr : '127.66.66.66'
}

db.logmsg.findOne()
{
  time : ISODate("2014-03-28T09:42:41.382Z"),
  message : 'cpu is on fire!',
  host: ObjectID('AAAB')  // Host 문서 참조
}

// 관련 로그 메시지 가져오기
host = db.hosts.findOne({ipaddr: '127.66.66.66'});
last_5k_msg = db.logmsg.find({host: host._id}).sort({time: -1}).limit(5000).toArray();
```

---

### 중급 개념
기본 설계 외에도 다음 사항을 고려할 수 있습니다:

- 양방향 참조를 사용하여 성능을 최적화하되, 원자적 업데이트가 불가능한 점을 감수해야 함.
- 데이터 비정규화는 "One" 측에서 "N" 측으로, 또는 그 반대 방향으로 수행 가능.

#### **양방향 참조**
- One-to-Many 관계에서 "One" 측과 "Many" 측 모두에 참조를 추가하여 성능 최적화.

```javascript
db.person.findOne()
{
    _id: ObjectID("AAF1"),
    name: "Kate Monster",
    tasks: [     // Task 문서의 참조 배열
        ObjectID("ADF9"), 
        ObjectID("AE02"),
        ObjectID("AE73") 
    ]
}

db.tasks.findOne()
{
    _id: ObjectID("ADF9"), 
    description: "Write lesson plan",
    due_date: ISODate("2014-04-01"),
    owner: ObjectID("AAF1")  // Person 문서 참조
}
```
- 단점: 데이터 갱신 시 두 곳을 모두 업데이트해야 함. 예: Task의 소유자를 변경하면 Person 문서와 Task 문서를 모두 수정해야 함.

#### **비정규화와 읽기 최적화**
- 비정규화를 통해 특정 필드를 중복 저장하여 읽기 성능을 향상.

```javascript
db.products.findOne()
{
    name: 'left-handed smoke shifter',
    manufacturer: 'Acme Corp',
    catalog_number: 1234,
    parts: [
        { id: ObjectID('AAAA'), name: '#4 grommet' },
        { id: ObjectID('F17C'), name: 'fan blade assembly' },
        { id: ObjectID('D2AA'), name: 'power switch' }
    ]
}
```
- 읽기 작업 간소화:
```javascript
product = db.products.findOne({catalog_number: 1234});
part_names = product.parts.map(part => part.name);
```
- 단점: 비정규화된 데이터가 업데이트될 경우, 모든 관련 문서를 수정해야 함.

---

### 비정규화와 선택의 다양성
MongoDB에서는 수천 가지 방법으로 스키마를 설계할 수 있지만, 너무 많은 선택지가 문제를 복잡하게 만듭니다.

#### 비정규화 설계 원칙:
1. 가능한 **임베드**를 선호하되, 합당한 이유가 있을 경우 참조 사용.
2. 데이터가 독립적으로 접근되어야 한다면 임베드 금지.
3. 배열 크기가 무한정 커지지 않도록 제한.
4. 어플리케이션 레벨 조인도 성능을 저하시키지 않으므로 활용.
5. 읽기-쓰기 비율을 고려. 읽기 위주의 필드는 비정규화에 적합.
6. 데이터 모델링은 어플리케이션의 데이터 액세스 패턴에 따라 결정.

---

### 관계 모델링
MongoDB에서 "One-to-N" 관계를 설계할 때 주요 고려사항:

1. **카디널리티**: "one-to-few", "one-to-many", "one-to-squillions".
2. N 측 데이터를 독립적으로 접근해야 하는가?
3. 읽기-쓰기 비율.

#### 주요 설계 옵션:
1. **one-to-few**: 임베드된 문서 배열.
2. **one-to-many** 또는 독립적인 N 측 필요: 참조 배열 사용 또는 N 측에 부모 참조 추가.
3. **one-to-squillions**: 부모 참조 사용.

비정규화는 자주 읽히고 드물게 업데이트되는 필드에 적합하며, 일관성이 덜 중요한 경우에만 사용.

---

### 유연성과 생산성
MongoDB는 어플리케이션의 요구에 맞게 데이터베이스 스키마를 설계할 수 있는 유연성을 제공합니다. 이를 통해 데이터 구조를 쉽게 변경할 수 있으며, 효율적인 쿼리와 업데이트를 지원하여 어플리케이션의 성능을 극대화할 수 있습니다.

