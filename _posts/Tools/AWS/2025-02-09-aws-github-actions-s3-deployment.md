---
title: "GitHub Actions를 활용한 S3 자동 배포 설정하기"
excerpt: "GitHub Actions를 이용해 AWS S3에 정적 웹사이트를 자동으로 배포하는 방법을 단계별로 설명합니다. IAM 사용자 설정, GitHub Secrets 등록, Actions YAML 파일 작성까지 차근차근 따라 해보세요."
categories:
  - AWS
  - DevOps
  - GitHub Actions
tags:
  - [AWS, S3, GitHub Actions, CI/CD, 자동 배포]
permalink: /aws/github-actions-s3-deployment/
toc: true
toc_sticky: true
date: 2025-02-09
last_modified_at: 2025-02-09
---

## 1. 개요

AWS S3를 정적 웹사이트 호스팅 용도로 사용할 때, 로컬에서 수동으로 파일을 업로드하는 것은 비효율적입니다. 이를 해결하기 위해 **GitHub Actions**를 활용하여 `git push`만으로 자동 배포가 가능하도록 설정하는 방법을 설명하겠습니다.

## 2. 사전 준비

### 2.1 S3 버킷 생성 및 정적 웹사이트 호스팅 설정
1. AWS 콘솔에서 **S3** 서비스 이동
2. `새 버킷 만들기` 클릭
   - 버킷 이름: `my-portfolio-bucket` (고유한 이름 설정 필요)
   - 리전: `ap-northeast-1` (도쿄 리전)
   - **퍼블릭 액세스 차단 설정 해제**
3. **정적 웹사이트 호스팅 활성화**
   - `속성 → 정적 웹사이트 호스팅` 활성화
   - `인덱스 문서`: `index.html`
   - `에러 문서`: `index.html` (SPA 사용 시)

### 2.2 IAM 사용자 생성 및 권한 부여
GitHub Actions가 AWS S3에 접근할 수 있도록 IAM 사용자를 생성해야 합니다.

1. AWS 콘솔에서 **IAM** 서비스 이동
2. **사용자(User) 추가**
   - 사용자 이름: `github-actions-s3`
   - 액세스 유형: **프로그래밍 방식 액세스** 체크
3. **권한 설정**
   - 기존 정책 직접 연결 → `AmazonS3FullAccess` 선택
4. 생성 후 **액세스 키 ID**와 **비밀 액세스 키**를 저장 (GitHub에 입력해야 함)

### 2.3 GitHub Secrets 등록
IAM 사용자 생성 시 받은 **액세스 키**를 GitHub에 등록합니다.

1. GitHub 저장소로 이동 (`your-repo-name`)
2. `Settings` → `Secrets and variables` → `Actions` → `New repository secret` 클릭
3. 다음 값을 추가
   - `AWS_ACCESS_KEY_ID` → `IAM 액세스 키 ID` 입력
   - `AWS_SECRET_ACCESS_KEY` → `IAM 비밀 액세스 키` 입력

## 3. GitHub Actions 설정

### 3.1 `.github/workflows/deploy.yml` 생성
GitHub 저장소에 Actions 설정 파일을 추가합니다.

```yaml
name: Deploy to S3

on:
  push:
    branches:
      - main  # main 브랜치에 푸시하면 자동 배포

jobs:
  deploy:
    runs-on: ubuntu-latest  # GitHub Actions 실행 환경

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Configure AWS CLI
        run: |
          aws configure set aws_access_key_id ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws configure set aws_secret_access_key ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws configure set region ap-northeast-1

      - name: Deploy to S3
        run: aws s3 sync ./ s3://my-portfolio-bucket --delete
```

### 3.2 동작 방식
✅ `on: push` → `main` 브랜치에 코드가 푸시될 때 실행
✅ `aws configure` → GitHub Actions가 AWS CLI를 설정
✅ `aws s3 sync` → 현재 프로젝트 파일을 S3 버킷과 동기화
✅ `--delete` → S3에서 삭제된 파일도 자동 삭제

## 4. 테스트 및 실행
1. **GitHub Actions 활성화 확인**
   - GitHub 저장소 `Actions` 탭에서 실행 여부 확인
   - `git push origin main` 실행 후 확인
2. **S3 버킷에서 파일 확인**
   - AWS 콘솔 → S3 → `my-portfolio-bucket` → `Objects` 확인
3. **웹사이트 접속 확인**
   - `http://my-portfolio-bucket.s3-website-ap-northeast-1.amazonaws.com/`

## 5. 추가 개선 사항

### 5.1 CloudFront 캐시 무효화
CloudFront를 사용하는 경우, 변경 사항이 즉시 반영되지 않을 수 있습니다. 캐시를 무효화하는 코드를 추가하세요.

```yaml
      - name: Invalidate CloudFront Cache
        run: aws cloudfront create-invalidation --distribution-id YOUR_DISTRIBUTION_ID --paths "/*"
```
✅ `YOUR_DISTRIBUTION_ID` → CloudFront 배포 ID 입력

### 5.2 특정 파일만 배포하기
폴더 전체가 아니라 특정 파일만 배포하려면 다음과 같이 수정합니다.

```yaml
      - name: Deploy only index.html
        run: aws s3 cp ./index.html s3://my-portfolio-bucket/
```

## 🎯 결론
✅ **`git push` 만으로 AWS S3 자동 배포 완료**  
✅ **GitHub Actions + AWS CLI 활용으로 효율적인 자동화**  
✅ **CloudFront 캐시 무효화까지 추가하면 완벽한 배포 환경 구성**  

이제 로컬에서 수동으로 배포할 필요 없이, 깃허브에 푸시하는 것만으로 자동으로 S3에 업로드됩니다! 🚀

