---
title: "AWS에서 Express.js 포트폴리오 배포 및 Google Domains 도메인 연결 방법"
excerpt: "AWS EC2 인스턴스를 사용하여 Express.js 포트폴리오 애플리케이션을 배포하고, Google Domains에서 구매한 도메인을 연결하는 방법을 자세히 설명합니다."
categories:
  - AWS
  - DevOps
  - Web Deployment
tags:
  - [AWS, EC2, Express.js, Node.js, Google Domains, 배포]
permalink: /aws/express-deploy/
toc: true
toc_sticky: true
date: 2025-02-04
last_modified_at: 2025-02-04
---

## Google Domains에서 도메인 구매

1. [Google Domains](https://domains.google/)에 접속하여 원하는 도메인을 구매합니다.
2. 도메인을 구매한 후 **DNS 관리**에 접근합니다.

---

## AWS에서 EC2 인스턴스 생성 및 설정

### 1️⃣ **EC2 인스턴스 생성**
1. [AWS 콘솔](https://aws.amazon.com/)에 로그인하고 **EC2 대시보드**로 이동합니다.
2. `Launch Instance` 클릭 → **Ubuntu AMI 선택**.
3. **인스턴스 유형**: `t2.micro` (무료 계층 사용 가능)
4. **키 페어 생성**: 다운로드 후 안전한 곳에 보관.
5. **보안 그룹 설정**:
   - HTTP(80), HTTPS(443), SSH(22) 포트 열기.
6. `Launch`를 클릭하여 인스턴스 시작.

### 2️⃣ **EC2에 SSH 접속 및 Node.js 설치**
```bash
ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-ip
sudo apt update
sudo apt install -y nodejs npm
node -v  # 설치 확인
npm -v  # 설치 확인
```

### 3️⃣ **Express.js 애플리케이션 배포**
```bash
git clone https://github.com/yourusername/your-repo.git
cd your-repo
npm install
npm start  # 또는 node server.js
```

애플리케이션이 실행되면 브라우저에서 `http://your-ec2-public-ip:3000`로 접속하여 확인합니다.

### 4️⃣ **PM2를 사용하여 백그라운드 실행**
```bash
sudo npm install -g pm2
pm2 start server.js
pm2 startup
```

---

## Google Domains에서 도메인 연결

### 1️⃣ **DNS 레코드 설정**
1. Google Domains → **DNS 관리** 이동.
2. `A 레코드` 추가:
   - **이름**: `www`
   - **레코드 유형**: `A`
   - **IP 주소**: `your-ec2-public-ip`
3. 변경사항 저장 후 최대 24시간 대기.

### 2️⃣ **도메인 연결 확인**
브라우저에서 `www.yourdomain.com`을 입력하여 Express.js 애플리케이션이 정상적으로 작동하는지 확인합니다.

---

## 추가 보안 설정 (SSL 인증서 적용)

### **Let's Encrypt SSL 인증서 적용 (HTTPS 설정)**
```bash
sudo apt install certbot
sudo certbot certonly --standalone -d yourdomain.com -d www.yourdomain.com
```

### **Nginx 리버스 프록시 설정 (옵션)**
```bash
sudo apt install nginx
sudo nano /etc/nginx/sites-available/default
```
아래 내용 추가:
```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
```bash
sudo systemctl restart nginx
```

---

## **마무리**
이제 `www.yourdomain.com`에서 배포한 Express.js 애플리케이션을 사용할 수 있습니다. 🚀

