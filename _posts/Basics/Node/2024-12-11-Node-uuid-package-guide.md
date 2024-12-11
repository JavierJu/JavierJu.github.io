---
title: "NPM의 UUID 패키지: 고유 식별자 생성과 사용법"
excerpt: "UUID란 무엇이며, NPM의 uuid 패키지를 사용하여 고유 식별자를 생성하는 방법을 알아봅니다. 다양한 UUID 버전과 설치 및 사용법도 함께 소개합니다."
categories:
  - Node
tags:
  - [JavaScript, Node.js, Backend, UUID, npm]
permalink: /Node/uuid-package-guide/
toc: true
toc_sticky: true
date: 2024-12-11
last_modified_at: 2024-12-11
---

### UUID란 무엇인가?
`UUID`는 **Universally Unique Identifier**의 약자로, 전 세계적으로 고유한 식별자를 생성하기 위한 표준입니다. UUID는 데이터베이스, 분산 시스템, 파일 이름, 세션 ID 등 고유성이 필요한 다양한 환경에서 사용됩니다.

이 글에서는 NPM의 [`uuid`](https://www.npmjs.com/package/uuid) 패키지를 이용해 UUID를 생성하고 사용하는 방법을 자세히 알아보겠습니다.

---

### UUID의 버전
`uuid` 패키지는 다양한 UUID 버전을 지원하며, 주요 버전은 아래와 같습니다.

- **v1**: 현재 시간과 호스트 장치의 하드웨어 정보를 기반으로 생성.
- **v3**: MD5 해시 알고리즘을 사용해 이름(namespace + name)을 기반으로 생성.
- **v4**: 난수 기반으로 생성.
- **v5**: SHA-1 해시 알고리즘을 사용해 이름(namespace + name)을 기반으로 생성.

---

### 설치 방법
`uuid` 패키지를 설치하려면 다음 명령을 실행합니다.

```bash
npm install uuid
```

---

### 사용법
#### 1. 난수 기반 UUID(v4)
난수를 기반으로 UUID를 생성하는 가장 일반적인 방식입니다.

```js
const { v4: uuidv4 } = require('uuid');

const id = uuidv4();
console.log(id);
// 예시 출력: 'b6a4c1e3-8492-44d7-83d8-7846e91f6e99'
```

#### 2. 시간 기반 UUID(v1)
시간 및 하드웨어 정보를 바탕으로 UUID를 생성합니다.

```js
const { v1: uuidv1 } = require('uuid');

const id = uuidv1();
console.log(id);
// 예시 출력: '2c975f00-78d3-11ec-90d6-0242ac120003'
```

#### 3. 이름 기반 UUID(v3, v5)
고정된 값을 기반으로 항상 동일한 UUID를 생성합니다. 이름과 네임스페이스를 사용하며, v3은 MD5, v5는 SHA-1 해시를 사용합니다.

```js
const { v3: uuidv3, v5: uuidv5 } = require('uuid');

// 네임스페이스를 생성 (네임스페이스는 고유해야 함)
const MY_NAMESPACE = uuidv4();

// v3: MD5 해싱
const idV3 = uuidv3('example.com', MY_NAMESPACE);
console.log(idV3);
// 예시 출력: '3a0f9bb5-5c75-3c95-9f6f-2dbef7ddcb76'

// v5: SHA-1 해싱
const idV5 = uuidv5('example.com', MY_NAMESPACE);
console.log(idV5);
// 예시 출력: 'e0736c95-c218-5392-943d-5c771a69b1d2'
```

#### 4. UUID 검증 및 버전 확인
생성된 UUID가 유효한지 확인하거나 UUID의 버전을 확인할 수 있습니다.

```js
const { validate, version } = require('uuid');

const id = uuidv4();

console.log(validate(id)); // true (유효한 UUID인지 확인)
console.log(version(id));  // 4 (UUID의 버전을 확인)
```

---

### UUID의 특징
- **고유성**: UUID는 매우 낮은 확률로 중복될 가능성이 있어 고유성을 보장합니다.
- **글로벌 식별자**: 네트워크 간에도 중복되지 않는 고유 ID를 제공합니다.
- **독립적 사용**: 중앙 서버 없이 로컬에서도 고유한 ID를 생성할 수 있습니다.

---

### 사용 사례
1. **데이터베이스**: 기본 키(PK)로 사용.
2. **분산 시스템**: 서로 다른 노드에서 고유 ID 생성.
3. **파일 이름**: 충돌을 방지하기 위한 고유 파일 이름 생성.
4. **API 토큰**: 사용자 세션 및 인증에 사용.
5. **트랜잭션 추적**: 로그에서 특정 작업을 식별.

---

`uuid` 패키지는 간단하면서도 강력한 도구로, 고유 식별자를 쉽게 생성할 수 있습니다. 다양한 버전과 사용 사례를 통해 여러분의 프로젝트에서 효율적으로 활용해 보세요!
