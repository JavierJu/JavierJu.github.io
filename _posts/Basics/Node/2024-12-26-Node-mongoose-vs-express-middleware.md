---
title: "Mongoose와 Express 미들웨어의 차이점: 데이터와 HTTP 요청 처리의 핵심"
excerpt: "Mongoose와 Express 미들웨어의 기능과 차이점을 비교하며, 각각의 사용 목적과 실행 흐름을 깊이 탐구합니다."
categories:
  - Node
tags:
  - [JavaScript, Express, Mongoose, Middleware, Backend]
permalink: /Node/mongoose-vs-express-middleware/
toc: true
toc_sticky: true
date: 2024-12-26
last_modified_at: 2024-12-26
---

## Mongoose 미들웨어

Mongoose 미들웨어는 MongoDB와 애플리케이션 간의 데이터 처리를 제어하거나 조작하기 위해 사용되는 함수들입니다. 데이터의 저장, 업데이트, 삭제 등 특정 동작이 발생하기 전후에 실행됩니다. 이를 통해 데이터 무결성 유지, 로깅, 유효성 검사 등의 작업을 쉽게 처리할 수 있습니다.

### **미들웨어의 종류**
Mongoose 미들웨어는 크게 두 가지로 나뉩니다:

1. **문서 미들웨어 (Document Middleware)**  
   - 개별 문서 작업에 대해 동작합니다.
   - `save`, `validate`, `remove`, `updateOne`, `deleteOne` 같은 특정 작업에 적용됩니다.
   - 예: 문서를 데이터베이스에 저장하기 전에 값을 수정하거나 검증.

   ```javascript
   const userSchema = new mongoose.Schema({
       name: String,
       email: String,
   });

   // 문서 미들웨어 - save
   userSchema.pre('save', function (next) {
       console.log('문서를 저장하기 전에 실행됩니다.');
       this.name = this.name.trim(); // name 속성을 정리
       next();
   });

   const User = mongoose.model('User', userSchema);
   ```

2. **쿼리 미들웨어 (Query Middleware)**  
   - `find`, `findOne`, `update`, `deleteMany` 등과 같은 쿼리에 대해 동작합니다.
   - 예: 특정 필드를 자동으로 제외하거나 데이터에 추가적인 조건을 추가.

   ```javascript
   userSchema.pre('find', function () {
       console.log('find 쿼리가 실행되기 전에 동작합니다.');
       this.where({ isActive: true }); // 활성 사용자만 조회
   });

   const User = mongoose.model('User', userSchema);
   ```

### **미들웨어의 실행 흐름**
- `pre`: 작업 **이전에** 실행됩니다.  
  `next()`를 호출하거나 프로미스를 반환해야 다음 단계로 진행됩니다.
- `post`: 작업 **이후에** 실행됩니다.  
  작업 완료 후 결과를 확인하거나 로깅 등에 사용됩니다.

   ```javascript
   userSchema.post('save', function (doc) {
       console.log(`${doc.name} 사용자가 저장되었습니다.`);
   });
   ```

### **비동기 코드 지원**
- `async/await`를 사용해 비동기 작업을 처리할 수 있습니다.

   ```javascript
   userSchema.pre('save', async function (next) {
       this.email = await formatEmail(this.email);
       next();
   });
   ```

---

## Express 미들웨어와의 차이점

### **1. 사용 목적**
- **Mongoose 미들웨어**: MongoDB 데이터 처리와 관련된 로직을 중앙 집중화하고 제어하기 위해 사용됩니다. 데이터의 무결성, 검증, 변경, 로깅 등이 주요 목적입니다.
- **Express 미들웨어**: HTTP 요청-응답 주기를 제어하고, 요청을 처리하거나 응답을 변경하는 데 사용됩니다. 라우팅, 인증, 에러 처리 등이 주요 목적입니다.

### **2. 동작의 컨텍스트**
- **Mongoose**: 데이터베이스 작업의 라이프사이클(`save`, `find` 등)에서 동작합니다.
- **Express**: 웹 서버의 요청-응답 라이프사이클(`GET`, `POST`, `PUT` 등)에서 동작합니다.

### **3. 실행 흐름**
- **Mongoose**: 데이터베이스 작업의 특정 시점(이전/이후)에 동작합니다.
- **Express**: 요청이 서버로 들어오고 나가는 동안 미들웨어 스택에서 순차적으로 실행됩니다.

### **4. 선언 위치**
- **Mongoose**: 스키마 정의 시 설정합니다.
- **Express**: 애플리케이션 레벨에서 `app.use()`로 등록합니다.

### **5. 예제 비교**
- **Mongoose 미들웨어**:
   ```javascript
   schema.pre('save', function (next) {
       this.createdAt = Date.now(); // 저장 전에 타임스탬프 추가
       next();
   });
   ```

- **Express 미들웨어**:
   ```javascript
   app.use((req, res, next) => {
       console.log(`${req.method} ${req.url} 요청`);
       next(); // 다음 미들웨어로 이동
   });
   ```

---

## 결론
Mongoose와 Express의 미들웨어는 둘 다 "특정 시점에서 작업을 처리"한다는 공통점이 있지만, 각각의 맥락과 사용 목적이 다릅니다.  
Mongoose는 데이터베이스와 관련된 로직을 캡슐화하고, Express는 HTTP 요청 처리 파이프라인을 관리합니다. 따라서 두 미들웨어를 함께 사용하면 데이터와 API 요청을 체계적으로 관리할 수 있습니다.

