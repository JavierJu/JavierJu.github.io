---
title: "AWS 인스턴스에서 MongoDB와 Mongoose 연결하기 - MongoDB 사용자 정보 설정 및 인증 활성화"
excerpt: "AWS EC2 인스턴스의 MongoDB를 설정하고 Mongoose를 통해 외부 애플리케이션에서 연결하는 방법을 단계별로 알아봅니다."
categories:
  - Tools
  
tags:
  - [AWS, MongoDB, Mongoose, Backend]
permalink: /Tools/mongodb-mongoose-connection/
toc: true
toc_sticky: true
date: 2024-12-19
last_modified_at: 2024-12-19
---

AWS EC2 인스턴스에서 MongoDB를 사용하려면 보안을 위해 사용자 계정을 설정하고 인증(Authorization)을 활성화해야 합니다. 이번 포스트에서는 다음을 다룹니다:

- MongoDB 사용자 계정 생성
- 인증 활성화 방법
- 문제 해결 과정

## 1. MongoDB 사용자 계정 생성

MongoDB의 인증 기능을 사용하려면 먼저 관리자 계정을 생성해야 합니다. `mongosh` 쉘에서 아래 명령어를 실행합니다:

```javascript
db.createUser({
  user: "admin",
  pwd: "your_password",
  roles: ["root"]
});
```

### 주요 설명
- `user`: 사용자 이름
- `pwd`: 비밀번호
- `roles`: 사용자 권한 설정. 여기서는 `root` 권한을 부여하여 모든 데이터베이스에 대한 접근 권한을 제공합니다.

> **Tip:** `your_password`는 강력한 비밀번호로 설정하세요.

## 2. MongoDB 인증 활성화

MongoDB의 인증을 활성화하려면 설정 파일(`/etc/mongod.conf`)을 수정해야 합니다. 다음과 같이 `security` 섹션을 추가합니다:

```yaml
security:
  authorization: enabled
```

### 설정 적용
설정을 저장한 후 MongoDB 서비스를 다시 시작합니다:

```bash
sudo systemctl restart mongod
```

이제 MongoDB는 인증이 활성화된 상태로 실행됩니다.

## 3. 문제 해결 과정

### A. `mongod` 서비스가 시작되지 않는 경우

설정 파일에 문제가 있는 경우 `mongod` 서비스가 시작되지 않을 수 있습니다. `sudo systemctl status mongod` 명령어로 상태를 확인하고 로그를 확인하세요:

```bash
sudo cat /var/log/mongodb/mongod.log
```

### B. `Unrecognized option: security` 오류

- `security` 섹션이 YAML 문법에 맞지 않게 작성된 경우 발생합니다.
- 공백과 들여쓰기를 확인하세요.
- `authorization: enabled`는 반드시 `security:` 아래에 작성되어야 합니다.

### C. 사용자 계정으로 접속하기

인증 활성화 후 MongoDB에 접속하려면 사용자 계정 정보를 제공해야 합니다:

```bash
mongosh --username admin --password your_password --authenticationDatabase admin
```

## 4. 참고 사항

- **XFS 파일 시스템**: MongoDB는 기본적으로 XFS 파일 시스템을 권장합니다. 그러나 테스트 환경에서는 ext4 파일 시스템도 무방합니다.
- **메모리 설정**: `/etc/security/limits.conf` 파일을 수정하여 적합한 메모리 설정을 적용하는 것을 고려하세요.

## 결론

이제 MongoDB가 인증 기능을 활성화한 상태로 안전하게 실행되고 있습니다. 다음 단계에서는 Mongoose를 사용하여 외부 애플리케이션에서 MongoDB에 연결하는 방법을 다뤄보겠습니다.

궁금한 점이나 문제가 있다면 댓글로 알려주세요!

