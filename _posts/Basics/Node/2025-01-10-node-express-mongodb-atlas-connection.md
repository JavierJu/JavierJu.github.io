---
title: "Express에서 MongoDB Atlas 연결하기: 클라우드 데이터베이스 활용 가이드"
excerpt: "MongoDB Atlas를 Express 애플리케이션과 연결하는 방법을 단계별로 알아보고, Mongoose를 활용한 데이터베이스 작업 예제를 제공합니다."
categories:
  - Node
  - Express
  - MongoDB
  - Backend
  - Web Development
tags:
  - [JavaScript, Express, MongoDB, Backend, 클라우드 데이터베이스]
permalink: /node/express-mongodb-atlas-connection/
toc: true
toc_sticky: true
date: 2025-01-10
last_modified_at: 2025-01-10
---

## Express에서 MongoDB Atlas 연결하기

MongoDB Atlas는 클라우드 기반의 데이터베이스 솔루션으로, 확장성과 관리의 편리함을 제공합니다. 이 문서에서는 Express 애플리케이션에서 MongoDB Atlas에 연결하는 방법을 단계별로 설명합니다. 또한 Mongoose를 활용하여 데이터베이스 작업을 효율적으로 처리하는 방법도 다룹니다.

---

### 1. MongoDB Atlas 계정 및 클러스터 생성

1. **MongoDB Atlas 가입**  
   [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)에 가입하고 계정을 만듭니다.

2. **클러스터 생성**  
   - Atlas 대시보드에서 "Create a Cluster" 버튼을 클릭하고 무료 클러스터를 생성합니다.
   - 클러스터를 설정한 후에는 몇 분 정도의 배포 시간이 필요합니다.

3. **데이터베이스 사용자 생성 및 네트워크 IP 설정**  
   - **Database Access** 메뉴에서 새로운 사용자(username, password)를 생성합니다.
   - **Network Access** 메뉴에서 IP 주소를 추가합니다.
     - 로컬 개발 환경에서는 "Add Current IP Address"를 클릭하거나 `0.0.0.0/0`을 입력해 모든 IP를 허용할 수 있습니다.

4. **MongoDB URI 가져오기**  
   - 클러스터 대시보드에서 "Connect" 버튼을 클릭합니다.
   - "Connect your application" 옵션을 선택한 후 URI를 복사합니다.
     URI는 다음과 같은 형식입니다:
     ```
     mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/<dbname>?retryWrites=true&w=majority
     ```

---

### 2. Express 프로젝트 설정

1. **Express 프로젝트 생성**  
   터미널에서 Express 프로젝트를 생성합니다.
   ```bash
   mkdir my-express-app
   cd my-express-app
   npm init -y
   npm install express mongoose dotenv
   ```

2. **프로젝트 구조**  
   프로젝트의 구조는 다음과 같이 설정할 수 있습니다:
   ```
   my-express-app/
   ├── node_modules/
   ├── .env
   ├── app.js
   ├── package.json
   ├── package-lock.json
   ```

---

### 3. 환경 변수 설정

1. 프로젝트 루트에 `.env` 파일을 생성하고 MongoDB URI를 추가합니다:
   ```env
   MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/<dbname>?retryWrites=true&w=majority
   ```

2. `.env` 파일을 안전하게 관리하기 위해 `.gitignore`에 추가합니다:
   ```
   .env
   ```

---

### 4. Express와 MongoDB 연결 코드 작성

1. **`app.js` 파일 생성 및 작성**
   ```javascript
   const express = require("express");
   const mongoose = require("mongoose");
   const dotenv = require("dotenv");

   // Load environment variables
   dotenv.config();

   const app = express();
   const PORT = 3000;

   // MongoDB 연결
   const connectDB = async () => {
       try {
           await mongoose.connect(process.env.MONGODB_URI, {
               useNewUrlParser: true,
               useUnifiedTopology: true,
           });
           console.log("MongoDB 연결 성공!");
       } catch (error) {
           console.error("MongoDB 연결 실패:", error);
           process.exit(1); // 실패 시 프로세스 종료
       }
   };

   // Middleware 설정
   app.use(express.json());

   // 라우트 설정
   app.get("/", (req, res) => {
       res.send("Hello, MongoDB with Express!");
   });

   // 서버 시작 및 DB 연결
   connectDB().then(() => {
       app.listen(PORT, () => {
           console.log(`서버가 포트 ${PORT}에서 실행 중입니다.`);
       });
   });
   ```

2. **MongoDB 연결 테스트**
   ```bash
   node app.js
   ```
   - "MongoDB 연결 성공!" 메시지가 나오면 MongoDB Atlas와의 연결이 완료된 것입니다.

---

### 5. 추가 팁

1. **Mongoose 스키마 및 모델 작성**  
   데이터베이스 작업을 위해 `Mongoose`의 스키마와 모델을 정의할 수 있습니다.  
   ```javascript
   const mongoose = require("mongoose");

   const userSchema = new mongoose.Schema({
       name: { type: String, required: true },
       email: { type: String, required: true, unique: true },
       age: { type: Number, required: false },
   });

   const User = mongoose.model("User", userSchema);

   module.exports = User;
   ```

2. **CRUD 작업 예시**
   ```javascript
   const User = require("./models/User");

   app.post("/users", async (req, res) => {
       try {
           const user = new User(req.body);
           await user.save();
           res.status(201).send(user);
       } catch (error) {
           res.status(400).send(error);
       }
   });
   ```

---

위의 내용을 따라 하면 Express에서 MongoDB Atlas에 안전하게 연결할 수 있습니다. 추가적인 질문이 있으면 언제든 물어보세요! 😊

