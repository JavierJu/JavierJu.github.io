---
title: "AWS 인스턴스에서 MongoDB와 Mongoose 연결하기"
excerpt: "AWS EC2 인스턴스의 MongoDB를 설정하고 Mongoose를 통해 외부 애플리케이션에서 연결하는 방법을 단계별로 알아봅니다."
categories:
  - Tools
  
tags:
  - [AWS, MongoDB, Mongoose, Backend]
permalink: /Tools/mongodb-mongoose-connection/
toc: true
toc_sticky: true
date: 2024-12-17
last_modified_at: 2024-12-17
---

AWS EC2 인스턴스의 MongoDB를 외부 애플리케이션에서 사용하려면, 몇 가지 설정이 필요합니다. 이 글에서는 **MongoDB Compass**와 **Mongoose**를 통해 MongoDB와 애플리케이션을 연결하는 과정을 설명합니다.

---

## 1. AWS 인스턴스의 IP 주소 확인하기
AWS 관리 콘솔에서 EC2 인스턴스의 **공인 IPv4 주소**를 확인합니다. 예를 들어, IP 주소가 `13.208.53.108`이라고 가정합니다.

---

## 2. MongoDB 외부 접속 허용 설정
AWS 인스턴스에 설치된 MongoDB가 외부에서 접속 가능하도록 설정 파일을 수정합니다.

### (1) MongoDB 설정 파일 열기
MongoDB의 설정 파일은 보통 `/etc/mongod.conf` 경로에 있습니다. 해당 파일을 엽니다.

```bash
sudo nano /etc/mongod.conf
```

### (2) `bindIp` 설정 수정
`bindIp` 항목을 `0.0.0.0`으로 변경하여 외부에서의 접속을 허용합니다.

```yaml
# 기존 설정
bindIp: 127.0.0.1

# 수정 후 설정
bindIp: 0.0.0.0
```

### (3) MongoDB 포트 확인
기본적으로 MongoDB는 **27017** 포트를 사용합니다. 특별히 변경하지 않았다면 그대로 사용하면 됩니다.

### (4) MongoDB 서비스 재시작
설정을 변경한 후 MongoDB를 재시작합니다.

```bash
sudo systemctl restart mongod
```

---

## 3. AWS 보안 그룹 설정
MongoDB 포트(기본: 27017)에 대한 인바운드 트래픽을 허용해야 합니다.

### (1) 보안 그룹 수정
1. AWS 관리 콘솔에서 EC2 인스턴스의 **보안 그룹(Security Group)**으로 이동합니다.
2. **인바운드 규칙**에 다음 항목을 추가합니다:
   - **타입**: Custom TCP
   - **포트 범위**: 27017
   - **소스**: `0.0.0.0/0` (모든 IP 허용) 또는 특정 IP 주소(더 안전).

---

## 4. Mongoose로 AWS MongoDB 연결하기
Node.js 애플리케이션에서 Mongoose를 사용하여 AWS MongoDB에 연결하려면 아래와 같이 작성합니다.

### (1) Mongoose 설치
Mongoose가 설치되어 있지 않다면 설치합니다.

```bash
npm install mongoose
```

### (2) Mongoose 연결 코드 작성
아래 코드는 Mongoose로 AWS MongoDB에 연결하는 예제입니다.

```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://13.208.53.108:27017/testDB', {
    useNewUrlParser: true,
    useUnifiedTopology: true
})
.then(() => console.log('Connected to AWS MongoDB!'))
.catch(err => console.error('Connection failed:', err));
```

---

## 5. 인증이 필요한 경우
MongoDB가 사용자 인증을 요구하는 경우, `username`과 `password`를 URL에 포함합니다.

```javascript
mongoose.connect('mongodb://username:password@13.208.53.108:27017/testDB', {
    useNewUrlParser: true,
    useUnifiedTopology: true
});
```

---

## 6. 보안 강화
MongoDB를 외부에 노출하는 것은 보안 위험이 있으므로 아래와 같은 조치를 취하는 것이 좋습니다.

1. **보안 그룹 규칙 제한**:
   - 특정 IP 주소만 접속 가능하도록 설정합니다.
2. **TLS/SSL 사용**:
   - `mongodb+srv://` 프로토콜을 사용해 TLS/SSL로 연결을 암호화합니다.
3. **VPN 사용**:
   - MongoDB를 VPN을 통해서만 접속 가능하도록 설정합니다.

---

위 단계를 따르면 AWS의 MongoDB에 Mongoose를 통해 안전하게 연결할 수 있습니다. 추가 질문이나 문제가 있으면 언제든지 댓글로 알려주세요! 😊

