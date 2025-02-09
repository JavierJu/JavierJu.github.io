---
title: "AWS Amplify Hosting vs S3 + CloudFront: 정적 웹사이트 호스팅 비교"
excerpt: "AWS Amplify Hosting과 S3 + CloudFront를 이용한 정적 웹사이트 호스팅의 차이점을 알아보고, 각각의 장단점과 활용 사례를 코드 예제와 함께 살펴봅니다."
categories:
  - AWS
  - Web Hosting
  - Cloud
  - DevOps
tags:
  - [AWS, Amplify, S3, CloudFront, 정적 웹사이트, 웹 호스팅]
permalink: /aws/amplify-vs-s3-cloudfront/
toc: true
toc_sticky: true
date: 2025-02-08
last_modified_at: 2025-02-08
---

AWS에서 정적 웹사이트를 호스팅하는 방법에는 여러 가지가 있지만, 대표적인 방식으로 **AWS Amplify Hosting**과 **Amazon S3 + CloudFront**가 있습니다. 이번 포스트에서는 두 가지 방식의 차이점을 비교하고, 어떤 경우에 어떤 방식을 선택하는 것이 좋은지 알아보겠습니다.

## 1. AWS Amplify Hosting이란?
AWS Amplify Hosting은 정적 웹사이트 및 풀스택 애플리케이션을 쉽고 빠르게 배포할 수 있는 **완전 관리형 호스팅 서비스**입니다. 특히 Git과 연동하여 **CI/CD(Continuous Integration & Continuous Deployment)** 기능을 제공하여 코드 변경 시 자동 배포가 가능합니다.

### ✅ **Amplify Hosting의 주요 특징**
- **CI/CD 지원**: GitHub, GitLab, Bitbucket과 연동하여 자동 배포 가능
- **자동 도메인 및 SSL 제공**: `*.amplifyapp.com` 기본 도메인 제공 및 HTTPS 자동 적용
- **서버리스 백엔드 통합**: AWS Lambda, DynamoDB, AppSync 등과 쉽게 연동 가능
- **CDN(CloudFront) 기본 포함**: 전 세계 배포로 빠른 로딩 속도 제공
- **A/B 테스트 및 프리뷰 배포 지원**: 새로운 기능을 미리 테스트할 수 있는 환경 제공

### ✅ **Amplify Hosting 배포 예제**
아래는 GitHub에서 가져온 React 애플리케이션을 AWS Amplify Hosting을 사용하여 배포하는 과정입니다.

```bash
# AWS CLI를 사용하여 Amplify 초기화
amplify init

# GitHub 리포지토리를 Amplify에 연결하고 빌드 및 배포 설정
amplify add hosting
amplify publish
```

## 2. S3 + CloudFront를 이용한 정적 웹사이트 호스팅
Amazon S3는 정적 웹사이트 호스팅을 지원하며, CloudFront는 CDN 역할을 수행하여 성능을 최적화합니다.

### ✅ **S3 + CloudFront 방식의 주요 특징**
- **S3 버킷을 통해 정적 파일 호스팅**
- **CloudFront를 이용한 글로벌 캐싱 및 HTTPS 지원**
- **Route 53을 사용하여 사용자 지정 도메인 연결 가능**
- **비용 절감 가능(Amplify 대비 상대적으로 저렴)**

### ✅ **S3 + CloudFront 배포 예제**
아래는 S3와 CloudFront를 이용해 정적 웹사이트를 배포하는 과정입니다.

```bash
# 정적 웹사이트 파일을 S3 버킷에 업로드
aws s3 mb s3://my-static-website
aws s3 sync ./dist s3://my-static-website --acl public-read

# CloudFront 배포 생성
aws cloudfront create-distribution --origin-domain-name my-static-website.s3.amazonaws.com
```

## 3. AWS Amplify Hosting vs S3 + CloudFront 비교

| 항목 | **AWS Amplify Hosting** | **S3 + CloudFront** |
|------|------------------------|---------------------|
| **배포 방식** | Git과 연동하여 자동 배포 | 수동으로 파일 업로드 및 배포 필요 |
| **CI/CD 지원** | 지원 (자동화된 빌드 및 배포) | 별도 설정(CodePipeline 등) 필요 |
| **도메인 설정** | 자동 설정 (`.amplifyapp.com`) | Route 53에서 직접 설정 필요 |
| **SSL/TLS 인증서** | 자동 적용 | ACM에서 수동 설정 필요 |
| **백엔드 연동** | AWS Lambda, DynamoDB, AppSync 등 쉽게 연동 | API Gateway, Lambda 설정 필요 |
| **CDN 포함 여부** | 기본 포함 | CloudFront 별도 설정 필요 |
| **비용** | 사용량 기반 과금(무료 요금제 포함) | 상대적으로 저렴하지만 트래픽 비용 고려 |

## 4. 어떤 방식을 선택해야 할까?
- **빠르게 배포하고 자동화된 CI/CD가 필요하다면 → AWS Amplify Hosting 추천** ✅
- **단순한 정적 웹사이트이고 비용을 최소화하고 싶다면 → S3 + CloudFront 추천** ✅
- **엔터프라이즈 환경에서 세밀한 설정과 확장이 필요하다면 → S3 + CloudFront + 추가 서비스 고려** ✅

## 5. 결론
AWS Amplify Hosting과 S3 + CloudFront는 각각의 강점이 있는 호스팅 솔루션입니다. 포트폴리오 웹사이트나 빠른 배포가 중요한 경우 **Amplify Hosting**이 더 편리하고, 비용을 아끼고 직접 세밀한 설정을 하고 싶다면 **S3 + CloudFront**가 좋은 선택이 될 수 있습니다.

**💡 여러분의 프로젝트에 맞는 최적의 호스팅 방식을 선택하세요! 🚀**

