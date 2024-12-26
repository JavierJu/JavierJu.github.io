---

title: "Express, MongoDB, Mongoose: 완벽 가이드" 
excerpt: "Express 서버와 MongoDB를 Mongoose로 연결하는 방법과 함께, 간단한 TODO 애플리케이션 예제를 통해 그 과정을 상세히 알아봅니다." 
categories:
- Node
Web Development tags:
- [JavaScript, Express, MongoDB, Mongoose, Node.js] 
permalink: /Node/express-mongo-mongoose-guide/ 
toc: true 
toc_sticky: true 
date: 2024-12-15
last_modified_at: 2024-12-15

---

## Express, MongoDB, Mongoose란?

Express, MongoDB, 그리고 Mongoose는 풀스택 JavaScript 개발에서 자주 사용되는 조합으로, 각각 다음의 역할을 합니다:

1. **Express**: Node.js 기반의 웹 애플리케이션 프레임워크로, HTTP 요청 및 응답을 처리하는 서버를 쉽게 구축할 수 있게 합니다.
2. **MongoDB**: NoSQL 데이터베이스로, 데이터를 JSON과 비슷한 BSON(Binary JSON) 형식으로 저장합니다.
3. **Mongoose**: MongoDB와 Node.js를 연결하는 ODM(Object Document Mapping) 라이브러리로, MongoDB 데이터를 더 구조화된 방식으로 관리하도록 도와줍니다.

---

## 연결 구조 및 사용 과정

1. **Express 설정**\
   Express는 웹 서버 역할을 하며, API 엔드포인트를 정의하여 클라이언트 요청을 처리합니다. 이 요청은 데이터베이스와 상호작용하거나 클라이언트로 데이터를 반환할 수 있습니다.

2. **MongoDB 연결**\
   MongoDB는 Mongoose를 통해 Express와 연결됩니다. Mongoose는 MongoDB와 상호작용하기 위해 `mongoose.connect()` 메서드를 사용합니다. 연결 정보에는 데이터베이스 URL과 옵션이 포함됩니다.

3. **Mongoose 모델 생성**\
   Mongoose를 사용하여 MongoDB 컬렉션을 정의하고 사용할 수 있는 모델을 생성합니다. 이 모델은 데이터 구조를 명확히 정의하며, 스키마 기반으로 동작합니다.

4. **Express 라우터와 데이터베이스 연결**\
   Express 라우터에서 요청을 처리할 때 Mongoose 모델을 사용하여 MongoDB 데이터를 읽거나 씁니다. 이를 통해 클라이언트의 요청을 처리하고 데이터를 반환합니다.

---

## 코드 예제: 간단한 TODO 애플리케이션

### 1. 기본 설정

``\*\* 파일:\*\*

```javascript
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

// Express 앱 생성
const app = express();
app.use(bodyParser.json()); // JSON 요청 처리

// MongoDB 연결
mongoose.connect('mongodb://localhost:27017/todoApp', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('MongoDB 연결 성공'))
  .catch(err => console.error('MongoDB 연결 실패:', err));

// 서버 포트
const PORT = 3000;
app.listen(PORT, () => {
  console.log(`서버가 http://localhost:${PORT} 에서 실행 중입니다.`);
});
```

---

### 2. Mongoose 스키마 및 모델 생성

``\*\* 파일:\*\*

```javascript
const mongoose = require('mongoose');

// 스키마 정의
const todoSchema = new mongoose.Schema({
  title: { type: String, required: true },
  completed: { type: Boolean, default: false }
});

// 모델 생성
const Todo = mongoose.model('Todo', todoSchema);

module.exports = Todo;
```

---

### 3. API 라우터 설정

``\*\* 파일:\*\*

```javascript
const express = require('express');
const Todo = require('../models/Todo'); // 모델 가져오기
const router = express.Router();

// 모든 TODO 조회
router.get('/', async (req, res) => {
  try {
    const todos = await Todo.find(); // MongoDB에서 데이터 조회
    res.json(todos);
  } catch (err) {
    res.status(500).json({ error: '데이터를 불러올 수 없습니다.' });
  }
});

// 새로운 TODO 추가
router.post('/', async (req, res) => {
  const { title } = req.body;
  try {
    const newTodo = new Todo({ title }); // 새로운 TODO 생성
    await newTodo.save(); // MongoDB에 저장
    res.status(201).json(newTodo);
  } catch (err) {
    res.status(400).json({ error: 'TODO를 생성할 수 없습니다.' });
  }
});

// 특정 TODO 삭제
router.delete('/:id', async (req, res) => {
  try {
    const todo = await Todo.findByIdAndDelete(req.params.id); // MongoDB에서 삭제
    if (!todo) return res.status(404).json({ error: 'TODO를 찾을 수 없습니다.' });
    res.json({ message: 'TODO가 삭제되었습니다.' });
  } catch (err) {
    res.status(500).json({ error: 'TODO를 삭제할 수 없습니다.' });
  }
});

module.exports = router;
```

---

### 4. 라우터를 Express에 추가

``\*\* 업데이트:\*\*

```javascript
const todoRoutes = require('./routes/todoRoutes');
app.use('/api/todos', todoRoutes);
```

---

## 실행 흐름 요약

1. **MongoDB 연결**: `mongoose.connect()`를 통해 Express 서버가 MongoDB에 연결됩니다.
2. **스키마 정의 및 모델 생성**: `Todo` 모델이 데이터베이스 컬렉션 구조를 정의합니다.
3. **Express 라우팅**: 클라이언트 요청(API 호출)을 처리하는 경로(`/api/todos`)를 정의합니다.
4. **Mongoose 작업**: API 요청에 따라 MongoDB에서 데이터를 읽거나 쓰는 작업을 수행합니다.

---

## 장점

- **Express**는 HTTP 요청 처리가 간편합니다.
- **MongoDB**는 JSON 스타일 데이터 저장에 적합하고 스키마 없이 유연합니다.
- **Mongoose**는 데이터 검증, 가공, 관계 설정 등 MongoDB 사용을 더 강력하게 만들어줍니다.

## 주요 참고 사항

- \*\*mongoose.connect()\*\*의 에러 처리를 반드시 포함하세요.
- Mongoose의 `Schema` 옵션으로 데이터 검증 규칙을 세밀하게 설정할 수 있습니다.
- MongoDB 연결 및 모델 작업은 비동기 방식으로 이루어지므로 `async/await` 또는 `Promise`를 활용하세요.

