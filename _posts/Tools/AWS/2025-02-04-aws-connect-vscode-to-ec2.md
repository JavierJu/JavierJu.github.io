---
title: "VS Code에서 AWS 인스턴스에 원격 접속하는 방법"
excerpt: "VS Code의 Remote-SSH 확장을 사용하여 AWS EC2 인스턴스에 원격 접속하는 방법을 단계별로 설명합니다. SSH 키 설정, 인스턴스 연결 및 보안 설정까지 포함합니다."
categories:
  - AWS
  - DevOps
  - Cloud
  - VSCode
  
tags:
  - [AWS, EC2, VSCode, Remote-SSH, 개발 환경, 서버 관리]
permalink: /aws/connect-vscode-to-ec2/
toc: true
toc_sticky: true
date: 2025-02-04
last_modified_at: 2025-02-04
---

AWS EC2 인스턴스를 VS Code에서 원격으로 관리하려면 **Remote-SSH 확장**을 활용하면 됩니다. 이 글에서는 SSH 키 설정부터 접속 방법까지 자세히 설명합니다.

---

## **1. EC2 인스턴스 설정 확인**
### ✅ 필요한 정보 확인
먼저 AWS EC2에 접속하기 위해 다음 정보를 준비하세요.
- **퍼블릭 IP**: EC2 인스턴스의 퍼블릭 IP 주소
- **SSH 키 파일 (`.pem`)**: EC2 접속을 위한 Private Key
- **기본 사용자 계정** (Amazon Linux: `ec2-user`, Ubuntu: `ubuntu`)
- **Security Group 설정**: SSH(포트 22) 접근이 허용되어 있는지 확인

---

## **2. VS Code에서 Remote-SSH 확장 설치**
1. VS Code를 열고 **Extensions (`Ctrl + Shift + X`)** 탭에서 `Remote - SSH`를 검색 후 설치합니다.
2. **Command Palette (`Ctrl + Shift + P`)** → `Remote-SSH: Connect to Host...` 선택
3. `Add New SSH Host...` 클릭 후 다음 형식으로 입력합니다.

```bash
ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-ip
```
> `ec2-user`는 Amazon Linux의 기본 계정이며, Ubuntu의 경우 `ubuntu`를 사용합니다.

4. 연결 후, VS Code에서 EC2 인스턴스의 파일을 직접 수정할 수 있습니다.

---

## **3. `.pem` 파일 안전하게 저장하기**
### ✅ `.pem` 파일 저장 위치 추천
보안상 **`~/.ssh/` 폴더**에 저장하는 것이 좋습니다.
```bash
mkdir -p ~/.ssh
mv ~/Downloads/my-key.pem ~/.ssh/
chmod 400 ~/.ssh/my-key.pem
```
Windows 사용자는 **`C:\Users\your-username\.ssh\`** 폴더에 저장하면 됩니다.

---

## **4. SSH 접속 및 테스트**
1. **터미널에서 직접 접속 확인**
```bash
ssh -i ~/.ssh/my-key.pem ec2-user@your-ec2-public-ip
```
2. 성공적으로 접속되면, 서버의 기본 상태를 확인합니다.
```bash
whoami
cat /etc/os-release
ip a
free -h
df -h
```

이제 AWS EC2 인스턴스를 VS Code에서 원격으로 접속하고 관리할 수 있습니다! 🚀

