---
title: "MongoDB 외부 연결 문제 해결 가이드: AWS 서버 설정과 Mongoose 사용"
excerpt: "AWS 인스턴스에서 MongoDB 외부 연결 문제를 진단하고 해결하는 방법을 단계별로 설명합니다. 보안 그룹, 방화벽, 그리고 Mongoose 설정을 확인하며 오류를 해결하세요."
categories:
  - Tools
tags:
  - [MongoDB, AWS, Node.js, Database, Troubleshooting]
permalink: /Tools/aws-mongodb-connection-guide/
toc: true
toc_sticky: true
date: 2024-12-19
last_modified_at: 2024-12-19
---

AWS 서버에서 MongoDB 외부 연결을 시도할 때 `ECONNREFUSED` 오류가 발생할 수 있습니다. 이 문서에서는 MongoDB와 AWS 설정을 점검하고 문제를 해결하는 방법을 단계별로 안내합니다.

## 문제 상황
MongoDB를 AWS 서버에서 실행하고 외부에서 Mongoose로 연결을 시도했지만 다음과 같은 오류가 발생했습니다:

```bash
$ node test.js
OH NO MONGO CONNECTION ERROR!
MongooseServerSelectionError: connect ECONNREFUSED hostIP:27017
```

## 해결 방법
### 1. MongoDB 외부 접속 허용 설정 확인
MongoDB가 외부 접속 요청을 수신할 수 있도록 설정을 확인해야 합니다.

#### 확인 및 조치:
1. **MongoDB 설정 파일 확인:**
   `/etc/mongod.conf` 파일에서 다음을 확인하세요:
   ```yaml
   net:
     port: 27017
     bindIp: 0.0.0.0
   ```

2. **MongoDB 재시작:**
   설정 변경 후 MongoDB를 재시작해야 적용됩니다:
   ```bash
   sudo systemctl restart mongod
   ```

3. **MongoDB 상태 확인:**
   ```bash
   sudo systemctl status mongod
   ```

---

### 2. AWS 보안 그룹 설정 확인
AWS 보안 그룹에서 27017 포트가 열려 있어야 외부에서 접속할 수 있습니다.

#### 확인 및 조치:
1. **보안 그룹 규칙 확인:**
   AWS Management Console에서 **인바운드 규칙**에 다음 항목이 포함되어 있는지 확인하세요:
   - 유형: 사용자 지정 TCP
   - 프로토콜: TCP
   - 포트 범위: 27017
   - 소스: `0.0.0.0/0`

2. **인스턴스 보안 그룹 연결 확인:**
   인스턴스 세부 정보에서 올바른 보안 그룹이 연결되어 있는지 확인합니다.

> **주의:** `0.0.0.0/0`은 모든 IP를 허용하므로 보안 위험이 있습니다. 고정 IP를 사용할 수 없는 경우 MongoDB 사용자 인증을 통해 보안을 강화하세요.

---

### 3. 서버 방화벽 (UFW) 확인
Ubuntu 서버에서 UFW를 사용하는 경우 27017 포트를 허용해야 합니다.

#### 확인 및 조치:
1. **UFW 상태 확인:**
   ```bash
   sudo ufw status
   ```

2. **27017 포트 허용:**
   ```bash
   sudo ufw allow 27017
   ```

---

### 4. 로컬 네트워크 테스트
외부 연결 문제를 진단하기 위해 AWS 외부에서 MongoDB 클라이언트를 이용해 연결을 테스트합니다.

#### 명령어 예시:
```bash
mongosh --host hostIP --port 27017 --username username --password password --authenticationDatabase admin
```

- 이 명령어로 성공적으로 연결된다면 MongoDB 설정은 문제가 없습니다.
- 연결이 실패하면 MongoDB 로그 (`/var/log/mongodb/mongod.log`)를 확인하세요.

---

### 5. Mongoose 연결 설정 확인
Mongoose로 연결할 때 다음 사항을 확인하세요:

1. **MongoDB URI 형식:**
   ```javascript
   mongoose.connect('mongodb://username:password@hostIP:27017/admin?authSource=admin')
   ```

2. **Mongoose 버전 확인:**
   최신 버전을 사용하고 있는지 확인합니다:
   ```bash
   npm install mongoose@latest
   ```

3. **디버그 활성화:**
   추가 로그를 확인하려면 디버그 모드를 활성화하세요:
   ```javascript
   mongoose.set('debug', true);
   ```

---

### 요약
위 단계를 따라 설정을 확인한 후에는 Mongoose와 MongoDB 간 연결이 정상적으로 이루어져야 합니다. 그래도 문제가 해결되지 않으면 MongoDB 로그를 분석하고 네트워크 연결 상태를 재점검하세요.

MongoDB 외부 접속은 보안이 중요한 작업이므로 항상 적절한 인증과 방화벽 설정을 병행해야 합니다.

