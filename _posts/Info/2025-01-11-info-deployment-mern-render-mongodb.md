---
title: "Render와 MongoDB Atlas를 활용한 MERN 스택 애플리케이션 배포"
excerpt: "Render와 MongoDB Atlas를 사용하여 MERN 스택 웹 애플리케이션을 배포하고, GitHub 연동을 통해 효율적으로 관리하는 방법을 자세히 알아봅니다."
categories:
  - Deployment
  - MERN
  - Web Development
tags:
  - [MERN, Render, MongoDB Atlas, GitHub, 배포]
permalink: /info/deployment-mern-render-mongodb/
toc: true
toc_sticky: true
date: 2025-01-11
last_modified_at: 2025-01-11
---

MERN 스택(MongoDB, Express, React, Node.js)을 Render와 MongoDB Atlas를 활용해 배포하는 방법을 단계별로 설명합니다. 이 과정에서 GitHub를 연동하여 프로젝트를 효율적으로 관리할 수 있도록 설정합니다.

---

## 1. 프로젝트 준비

### 1.1 MERN 스택 애플리케이션 준비하기

- **백엔드**: `Node.js`와 `Express`로 API 서버 개발.
- **프론트엔드**: `React`로 클라이언트 애플리케이션 구축.
- **데이터베이스**: MongoDB Atlas를 데이터 저장소로 사용.

#### 환경 변수 관리하기
프로젝트 루트 디렉토리에 `.env` 파일을 생성하고 중요한 정보를 관리합니다:

```env
MONGO_URI=mongodb+srv://<username>:<password>@<cluster-url>/<database>?retryWrites=true&w=majority
JWT_SECRET=your_jwt_secret
PORT=4000
```

### 1.2 GitHub에 코드 업로드
1. 로컬 프로젝트 디렉토리에서 GitHub에 코드를 업로드합니다:

```bash
git init
git remote add origin <your-repository-URL>
git add .
git commit -m "Initial commit"
git push -u origin main
```

2. Render에서 해당 리포지토리를 사용해 배포할 예정입니다.

---

## 2. MongoDB Atlas 설정

1. **MongoDB Atlas 계정 생성 및 클러스터 설정**
   - [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)에 로그인 후 클러스터를 생성합니다.
   - 클러스터 생성 후 데이터베이스 사용자와 비밀번호를 설정합니다.
   - IP 접근 제한 설정: `0.0.0.0/0`로 설정하여 모든 IP를 허용하거나, 특정 IP만 허용.

2. **연결 URI 복사**
   - 클러스터의 "Connect" 버튼을 클릭 → "Application"을 선택 → 연결 URI 복사:

```text
mongodb+srv://<username>:<password>@<cluster-url>/<database>?retryWrites=true&w=majority
```

---

## 3. Render에서 백엔드 서버 배포

### 3.1 Render에서 Node.js 서비스 생성

1. [Render](https://render.com)에 로그인하고 **New** → **Web Service**를 클릭합니다.
2. GitHub 리포지토리를 연동하고 배포할 리포지토리를 선택합니다.
3. 설정값을 입력합니다:

   - **Name**: 서비스 이름 지정.
   - **Environment**: `Node`
   - **Build Command**:
     ```bash
     npm install
     ```
   - **Start Command**:
     ```bash
     npm start
     ```
   - **Environment Variables**:
     - `MONGO_URI`: MongoDB Atlas에서 복사한 연결 URI.
     - 추가로 필요한 환경 변수도 여기에 입력합니다.

4. "Create Web Service" 버튼을 눌러 배포를 시작합니다.

---

## 4. Render에서 프론트엔드 배포

### 4.1 React 애플리케이션 배포

1. Render에서 다시 **New** → **Static Site**를 선택합니다.
2. GitHub 리포지토리를 선택합니다.
3. 설정값을 입력합니다:

   - **Build Command**:
     ```bash
     npm install && npm run build
     ```
   - **Publish Directory**:
     ```text
     build
     ```

4. "Create Static Site" 버튼을 눌러 배포를 시작합니다.

---

## 5. 프론트엔드와 백엔드 연결

1. React 애플리케이션에서 환경 변수로 백엔드 API URL 설정:
   - `.env` 파일에 다음과 같이 추가:
     ```env
     REACT_APP_API_URL=https://<your-backend-service>.onrender.com
     ```
   - React 애플리케이션을 다시 빌드 후 배포합니다.

2. 백엔드 서버에서 CORS 설정:
   - `cors` 패키지를 설치하고 React 앱의 도메인을 허용:

     ```javascript
     const cors = require('cors');
     app.use(cors({
       origin: 'https://<your-frontend-service>.onrender.com',
     }));
     ```

---

## 6. 배포 테스트

1. 프론트엔드 URL에서 애플리케이션 접속.
2. MongoDB Atlas와의 연결, 백엔드 API 호출이 정상적으로 작동하는지 확인합니다.

---

## 7. 추가 최적화 및 관리

- **자동 배포 설정**:
  - Render에서 GitHub 리포지토리의 `main` 브랜치를 추적하도록 설정하여 코드 업데이트 시 자동 배포.

- **로그 관리**:
  - Render 대시보드에서 서비스 로그를 확인해 오류를 분석.

- **문제 해결**:
  - Render 대시보드에서 환경 변수를 다시 확인.
  - MongoDB Atlas의 연결 설정(IP, 사용자 계정 등)을 점검.

---

이 가이드를 통해 MERN 스택 애플리케이션을 Render와 MongoDB Atlas로 성공적으로 배포하고 관리할 수 있습니다. 필요시 추가적인 도움을 요청하세요! 😊

