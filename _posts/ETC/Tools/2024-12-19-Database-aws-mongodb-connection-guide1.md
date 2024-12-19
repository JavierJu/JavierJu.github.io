---
title: "AWS MongoDB 접속 방법 정리"
excerpt: "AWS 인스턴스에서 MongoDB에 접속하는 다양한 방법을 정리합니다. 내부 터미널, 외부 터미널, Express-Mongoose, Compass, SSH 터널링 등 다양한 접근 방식을 다룹니다."
categories:
  - Tools
tags:
  - [AWS, MongoDB, Database, Node.js, Backend]
permalink: /Tools/aws-mongodb-connection-guide1/
toc: true
toc_sticky: true
date: 2024-12-19
last_modified_at: 2024-12-19
---

AWS 인스턴스에서 MongoDB에 접속하는 방법을 정리했습니다. 다양한 접근 방식을 상황에 맞게 활용할 수 있도록 내부/외부 터미널, Express-Mongoose, GUI 도구, SSH 터널링 방법을 포함했습니다.

---

### 1. 내부 터미널에서 접속
AWS 인스턴스 내부에서 MongoDB에 직접 접속하는 방법입니다:
```bash
$ mongosh --username username --password password --authenticationDatabase admin
```

---

### 2. 외부 터미널에서 접속
AWS 인스턴스 외부에서 MongoDB에 접속하려면 퍼블릭 IP와 포트를 지정합니다:
```bash
$ mongosh --host hostIP --port 27017 --username username --password password --authenticationDatabase admin
```

---

### 3. Express-Mongoose를 사용한 접속
Node.js 애플리케이션에서 Mongoose를 통해 MongoDB에 연결하는 예제입니다:
```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://username:password@hostIP:27017/farmStand?authSource=admin')
    .then(() => {
        console.log("MONGO CONNECTION OPEN");
    })
    .catch(err => {
        console.log('OH NO MONGO CONNECTION ERROR!');
        console.log(err);
    });
```

---

### 4. MongoDB Compass 사용
MongoDB의 GUI 도구인 Compass를 사용하여 데이터베이스를 관리할 수 있습니다:
- **Connection String**:
  ```plaintext
  mongodb://username:password@hostIP:27017/admin
  ```
- **Authentication Database**: `admin`

Compass를 사용하면 데이터베이스 구조를 시각적으로 관리하고, 쿼리를 테스트하기에 편리합니다.

---

### 5. CLI를 통한 데이터베이스 명령 실행
MongoDB CLI를 사용해 명령어 기반으로 데이터베이스를 관리할 수 있습니다:
```bash
$ mongo --host hostIP --port 27017 -u username -p password --authenticationDatabase admin
```

---

### 6. SSH 터널링을 통한 보안 접속
SSH 터널링을 사용하면 로컬 네트워크에서 안전하게 MongoDB에 접근할 수 있습니다:
1. **SSH 포트 포워딩 설정**:
   ```bash
   $ ssh -L 27017:localhost:27017 ubuntu@hostIP
   ```
2. **로컬에서 MongoDB 접속**:
   ```bash
   $ mongosh --host localhost --port 27017 --username username --password password --authenticationDatabase admin
   ```

---

### 추가 고려 사항
- **보안**: MongoDB 접속 시 사용자 인증 정보를 안전하게 관리해야 하며, `.env` 파일에 저장하거나 환경 변수를 사용하는 것이 좋습니다.
- **IP 제한**: 외부에서 접속을 허용할 경우, 보안 그룹의 IP 범위를 최소화하거나 VPN을 활용하는 것이 권장됩니다.
- **VPN/Proxy 활용**: 네트워크 환경에 따라 VPN이나 프록시를 사용해 추가적인 보안을 확보할 수 있습니다.

이 가이드를 활용하여 AWS에서 MongoDB를 효과적으로 관리하고 안전하게 접근하세요.

