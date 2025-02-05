---
title: "AWS 무료 티어를 활용한 포트폴리오 배포 및 HTTPS 설정"
excerpt: "AWS 무료 티어에서 EC2, Route 53, 로드 밸런서, CloudFront, WAF 등을 활용하여 Express 기반 포트폴리오 웹사이트를 배포하고 HTTPS를 적용하는 방법을 자세히 설명합니다."
categories:
  - AWS
  - DevOps
  - Web Deployment
tags:
  - [AWS, EC2, Route 53, CloudFront, HTTPS, SSL, Express]
permalink: /aws/deploy-portfolio-https/
toc: true
toc_sticky: true
date: 2025-02-05
last_modified_at: 2025-02-05
---

## 개요
AWS 무료 티어를 활용하여 포트폴리오 웹사이트를 배포하고, HTTPS를 적용하는 방법을 정리했습니다. 이 글에서는 EC2 인스턴스 설정부터 Route 53을 이용한 도메인 연결, 로드 밸런서와 CloudFront 설정, 그리고 보안 강화를 위한 WAF 적용까지 다룹니다.

## 1. AWS EC2에 포트폴리오 배포
### 1.1 EC2 인스턴스 생성
1. AWS 콘솔에서 **EC2 인스턴스**를 생성합니다.
2. **Amazon Linux 2** 또는 **Ubuntu** AMI를 선택합니다.
3. 무료 티어를 유지하기 위해 **t2.micro** 인스턴스를 선택합니다.
4. 보안 그룹에서 **80번(HTTP), 443번(HTTPS), 22번(SSH)** 포트를 엽니다.
5. 생성된 인스턴스에 SSH로 접속합니다.

```sh
ssh -i your-key.pem ec2-user@your-ec2-ip
```

### 1.2 Node.js 및 Express 앱 배포
1. 서버에서 Node.js를 설치합니다.

```sh
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo yum install -y nodejs  # Amazon Linux
```

2. Express 기반의 포트폴리오를 배포합니다.

```sh
git clone https://github.com/your-repo/portfolio.git
cd portfolio
npm install
npm run build  # 프론트엔드 빌드 (React/Vue 등 사용 시)
npm start
```

3. **PM2**를 사용해 서버를 백그라운드에서 실행합니다.

```sh
npm install -g pm2
pm2 start server.js --name portfolio
pm2 save
pm2 startup
```

## 2. 도메인 연결 (Route 53)
### 2.1 도메인 구매 및 설정
1. **AWS Route 53**에서 도메인을 구매합니다.
2. 호스팅 영역(Hosted Zone)을 생성하고, **A 레코드**를 추가하여 EC2의 퍼블릭 IP를 연결합니다.
3. 서브도메인(www)도 함께 설정하려면 **CNAME 레코드**를 추가합니다.

## 3. HTTPS 적용 (SSL/TLS 인증서)
### 3.1 AWS Certificate Manager (ACM)에서 SSL 인증서 생성
1. AWS ACM에서 **도메인 검증 방식으로 SSL 인증서**를 생성합니다.
2. Route 53에서 자동 검증을 선택하여 인증서를 활성화합니다.

## 4. 로드 밸런서(ALB) 설정
### 4.1 ALB 생성 및 EC2 연결
1. **Application Load Balancer(ALB)**를 생성합니다.
2. **대상 그룹(Target Group)**을 생성하고 EC2 인스턴스를 추가합니다.
3. 리스너 설정에서 **HTTP(80) -> HTTPS(443) 리디렉션** 규칙을 추가합니다.

## 5. CloudFront 설정
### 5.1 CloudFront 배포 생성
1. **CloudFront 배포**를 생성하고, 원본(origin)으로 **ALB를 설정**합니다.
2. HTTPS를 강제 적용하고, 캐싱 정책을 설정합니다.
3. SSL 인증서를 적용하여 보안을 강화합니다.

## 6. Express에서 HTTPS 및 도메인 리디렉션 설정
### 6.1 HTTPS 강제 리디렉션
```js
app.use((req, res, next) => {
  if (!req.secure) {
    return res.redirect('https://' + req.headers.host + req.url);
  }
  next();
});
```

### 6.2 www 서브도메인으로 리디렉션
```js
app.use((req, res, next) => {
  if (req.headers.host === 'javierju.com') {
    return res.redirect(301, 'https://www.javierju.com' + req.url);
  }
  next();
});
```

## 7. 보안 강화 (AWS WAF 설정)
### 7.1 WAF 웹 ACL 설정
1. AWS WAF에서 **Web ACL**을 생성합니다.
2. SQL Injection, XSS 공격 방어 규칙을 추가합니다.
3. Web ACL을 CloudFront와 ALB에 연결합니다.

## 8. 결론
이제 AWS 무료 티어를 활용하여 포트폴리오를 HTTPS로 배포하고 보안을 강화하는 작업을 완료했습니다. 앞으로 CloudFront 캐싱 최적화 및 S3 연동도 도전해볼 계획입니다.

