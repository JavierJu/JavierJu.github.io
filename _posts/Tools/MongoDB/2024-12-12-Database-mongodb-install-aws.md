---
title: "AWS EC2 Ubuntu Instance에 MongoDB 설치하기"
excerpt: "AWS EC2 Ubuntu 인스턴스에 MongoDB를 설치하는 방법을 단계별로 안내합니다. MySQL과 함께 병렬로 사용할 수 있습니다."
categories:
  - MongoDB
tags:
  - [AWS, EC2, MongoDB, Ubuntu]
permalink: /mongodb/mongodb-install-aws/
toc: true
toc_sticky: true
date: 2024-12-12
last_modified_at: 2024-12-12
---

AWS EC2 인스턴스에 MongoDB를 설치하는 방법을 단계별로 설명합니다. MySQL이 이미 설치되어 있어도 문제 없이 MongoDB를 추가로 설치할 수 있습니다.

---

## 1. MongoDB 저장소 추가
MongoDB 패키지를 공식 MongoDB 저장소에서 설치하려면 저장소를 추가해야 합니다.

```bash
# 시스템 패키지 업데이트
sudo apt update

# 필요한 패키지 설치
sudo apt install -y wget gnupg

# MongoDB 공개 GPG 키 추가
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -

# MongoDB 저장소 추가
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu $(lsb_release -sc)/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```

---

## 2. MongoDB 설치
MongoDB 서버를 설치합니다.

```bash
# 패키지 리스트 업데이트
sudo apt update

# MongoDB 설치
sudo apt install -y mongodb-org
```

---

## 3. MongoDB 서비스 시작 및 활성화
설치가 완료되면 MongoDB를 시작하고 부팅 시 자동으로 시작되도록 설정합니다.

```bash
# MongoDB 서비스 시작
sudo systemctl start mongod

# 부팅 시 자동 시작 활성화
sudo systemctl enable mongod

# 서비스 상태 확인
sudo systemctl status mongod
```

---

## 4. 설치 확인
MongoDB가 제대로 설치되었는지 확인하려면 MongoDB 셸을 실행해 봅니다.

```bash
mongo --eval 'db.runCommand({ connectionStatus: 1 })'
```

---

## 5. 포트 및 보안 설정 (선택 사항)
기본적으로 MongoDB는 포트 `27017`에서 실행됩니다. 외부 접근을 허용하려면 **MongoDB 구성 파일(`/etc/mongod.conf`)**을 수정해야 합니다.

- 외부 접근 허용:
  - `bindIp` 항목에서 `127.0.0.1` 대신 `0.0.0.0` 추가.
- 수정 후 서비스 재시작:
  ```bash
  sudo systemctl restart mongod
  ```

**주의:** 외부 접근을 허용할 경우 방화벽 설정이나 인증 메커니즘을 추가로 구성해야 보안을 유지할 수 있습니다.

---

이후 MySQL과 MongoDB를 병렬로 사용하면서 각각의 용도에 맞게 활용하면 됩니다. MongoDB는 NoSQL 데이터베이스로 JSON과 유사한 문서를 처리하는 데 적합하며, MySQL은 관계형 데이터베이스로 구조화된 데이터를 저장하는 데 적합합니다.
