---
title: "AWS Route 53에서 www 없는 도메인을 www 도메인으로 리디렉션하기"
excerpt: "AWS Route 53에서 javierju.com을 www.javierju.com으로 리디렉션하는 방법을 설명합니다. S3 정적 웹사이트, CloudFront, ACM 인증서를 활용하여 HTTPS 리디렉션까지 설정하는 방법을 코드 예제와 함께 정리했습니다."
categories:
  - AWS
  - Cloud
  - DevOps
tags:
  - [AWS, Route 53, CloudFront, S3, HTTPS, 도메인 리디렉션]
permalink: /aws/route53-www-redirect/
toc: true
toc_sticky: true
date: 2025-02-05
last_modified_at: 2025-02-05
---

AWS Route 53에서 `javierju.com`을 `www.javierju.com`으로 리디렉션하는 방법을 정리합니다. 일반적으로 두 가지 방법이 있습니다.

1. **S3 정적 웹사이트 + Route 53** (간단한 HTTP 리디렉션)
2. **S3 + CloudFront + ACM 인증서 + Route 53** (HTTPS 포함 리디렉션)

이 글에서는 HTTPS까지 지원하는 **CloudFront를 활용한 방법**을 자세히 설명하겠습니다.

---

## 🔹 1. S3에서 리디렉션 버킷 생성
1. **S3 콘솔에서 새 버킷 생성**
   - 버킷 이름: `javierju.com` (도메인과 정확히 일치해야 함)
   - 퍼블릭 액세스 차단 설정: 유지 (CloudFront를 사용하므로 퍼블릭 접근 필요 없음)

2. **정적 웹사이트 호스팅 활성화**
   - "속성(Properties)" → "정적 웹 사이트 호스팅(Static website hosting)"
   - "리디렉션 요청을 다른 도메인으로 전달" 선택
   - 대상 도메인: `www.javierju.com`
   - 프로토콜: `https`

---

## 🔹 2. ACM에서 SSL/TLS 인증서 발급 (버지니아 북부 리전)
CloudFront에서 HTTPS를 사용하려면 **버지니아 북부(us-east-1)에서 인증서를 발급**해야 합니다.

1. **AWS Certificate Manager(ACM) 이동** ([바로 가기](https://console.aws.amazon.com/acm/home?region=us-east-1))
2. 리전 선택 → **버지니아 북부(us-east-1)**
3. "인증서 요청" 클릭 → "퍼블릭 인증서 요청"
4. 도메인 입력: `javierju.com`, `www.javierju.com` 추가
5. 검증 방법: **DNS 검증** 선택
6. Route 53에서 자동으로 DNS 레코드 추가하여 검증 완료

---

## 🔹 3. CloudFront 배포 설정
1. **CloudFront 콘솔에서 새 배포 생성**
   - 원본 도메인: `javierju.com.s3-website.<region>.amazonaws.com`
   - 뷰어 프로토콜 정책(Viewer Protocol Policy): `Redirect HTTP to HTTPS`
   - 기본 동작 설정: `Managed-CachingDisabled`
   - CNAME 추가: `javierju.com`
   - SSL/TLS 인증서: **버지니아 북부에서 발급한 인증서 선택**

2. CloudFront 배포 완료 후 제공된 **도메인 이름 (예: `d123abc.cloudfront.net`) 복사**

---

## 🔹 4. Route 53에서 A 레코드 추가 (CloudFront Alias 연결)
1. Route 53 → `javierju.com` 호스팅 영역 이동
2. **새 레코드 추가**
   - 레코드 유형: `A - IPv4 address`
   - **Alias 활성화**
   - 대상: **CloudFront 배포 선택**

✅ 설정이 완료되면 `javierju.com`으로 접속 시 `www.javierju.com`으로 HTTPS 리디렉션됩니다.

---

## 🔹 5. Express 서버에서 직접 리디렉션 설정하는 방법 (선택 사항)
만약 **EC2에서 Express 서버를 운영 중이라면**, 코드로 직접 리디렉션할 수도 있습니다.

```javascript
const express = require("express");
const app = express();

app.use((req, res, next) => {
  if (req.hostname === "javierju.com") {
    return res.redirect(301, "https://www.javierju.com" + req.originalUrl);
  }
  next();
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

✅ 이 방법은 CloudFront 없이 **서버에서 직접 리디렉션을 처리**하는 방식입니다.

---

## 🚀 결론
- Route 53만으로는 리디렉션이 불가능하므로 **S3 또는 CloudFront를 활용해야 함**
- HTTP 리디렉션만 필요하면 **S3 정적 웹사이트 + Route 53 Alias** 사용
- HTTPS까지 포함하려면 **CloudFront + ACM(버지니아 북부) + Route 53** 사용
- Express 서버를 운영 중이라면 **301 리디렉션 코드**를 추가하여 직접 처리 가능

이제 `javierju.com`을 `www.javierju.com`으로 안전하게 리디렉션할 수 있습니다! 🚀

