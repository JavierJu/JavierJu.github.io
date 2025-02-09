---

title: "AWS S3 정적 웹사이트 호스팅: EC2에서 배포하고 Express는 API 서버로 전환"
excerpt: "AWS S3를 활용한 정적 웹사이트 호스팅 방법을 설명합니다. EC2에서 S3로 정적 파일을 배포하고, Express 서버를 API 전용으로 전환하는 과정을 코드 예제와 함께 소개합니다."
categories:

- AWS
- Cloud
- DevOps
- Web Hosting

tags:

- [AWS, S3, 정적 웹사이트, 배포, 웹 호스팅, Express, API]
  permalink: /aws/s3-static-web-hosting/
  toc: true
  toc_sticky: true
  date: 2025-02-08
  last_modified_at: 2025-02-08

---

# AWS S3 정적 웹사이트 호스팅: EC2에서 배포하고 Express는 API 서버로 전환

AWS S3를 이용하면 정적 웹사이트를 손쉽게 호스팅할 수 있습니다. 이번 글에서는 **EC2에서 AWS CLI를 사용하여 S3로 정적 파일을 업로드**하는 방법과 Express 서버를 API 전용으로 전환하는 과정을 소개합니다.

## 1. AWS CLI 설치 및 설정

### 1.1 AWS CLI 설치 확인

EC2에서 AWS CLI가 설치되어 있는지 확인하려면 터미널에서 다음 명령어를 실행하세요.

```sh
aws --version
```

출력 예시:

```sh
aws-cli/2.11.9 Python/3.9.6 Linux/x86_64
```

### 1.2 AWS 자격 증명 설정 (IAM 역할 적용)

EC2에서 S3에 직접 접근하려면 **IAM 역할을 부여**하는 것이 좋습니다.

1. AWS 콘솔에서 EC2 인스턴스에 접속
2. IAM 역할 생성 (S3 접근 권한 포함)
3. EC2 인스턴스에 IAM 역할 할당

이렇게 설정하면 `aws configure`로 액세스 키를 직접 입력하지 않아도 됩니다.

## 2. S3 버킷 생성 및 정적 웹사이트 호스팅 설정

### 2.1 S3 버킷 생성

```sh
aws s3 mb s3://www.javierju.com --region ap-northeast-1
```

### 2.2 정적 웹사이트 호스팅 활성화

1. **AWS 콘솔에서 S3 이동**
2. **생성한 버킷 선택**
3. **[속성] 탭 → 정적 웹사이트 호스팅**
4. **[활성화] 체크 후, 인덱스 문서(`index.html`) 설정**
5. **저장**

### 2.3 퍼블릭 액세스 허용 및 버킷 정책 설정

```sh
aws s3api put-bucket-policy --bucket www.javierju.com --policy file://policy.json
```

**`policy.json` 예시:**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::www.javierju.com/*"
    }
  ]
}
```

## 3. EC2에서 S3로 정적 파일 업로드

### 3.1 정적 파일 업로드 명령어

```sh
aws s3 sync public/ s3://www.javierju.com --delete
```

이제 S3가 정적 파일을 직접 서빙하므로, EC2에서는 더 이상 정적 파일을 제공할 필요가 없습니다.

## 4. Express 서버를 API 전용으로 변경

### 4.1 기존 정적 파일 서빙 코드 제거

**변경 전:**
```js
app.use(express.static("public"));
```

**변경 후 (API 전용 서버로 전환):**
```js
app.use(express.json());
app.use("/api", apiRoutes);
```

## 5. S3 웹사이트 접속 테스트

정상적으로 설정되었다면, 아래 URL로 접속하면 정적 웹사이트가 열립니다.

```sh
http://www.javierju.com.s3-website-ap-northeast-1.amazonaws.com
```

## 6. AWS Route 53에서 구입한 도메인 연결

### 6.1 Route 53에서 호스팅 영역 생성
1. AWS 콘솔 → **Route 53** 이동
2. **호스팅 영역(Hosted Zone) 생성** 클릭
3. 도메인 이름: `www.javierju.com` 입력 후 생성

### 6.2 S3 정적 웹사이트를 Route 53 도메인에 연결
1. Route 53 → **도메인 선택**
2. **A 레코드 추가**
   - **레코드 유형**: A (IPv4 주소)
   - **Alias(별칭) 활성화**
   - **S3 버킷 선택**
   - 저장

이제 `www.javierju.com`으로 접속하면 S3에 업로드된 정적 사이트로 이동하게 됩니다.

## 7. CloudFront를 이용한 HTTPS 적용 (선택 사항)

CloudFront를 설정하여 HTTPS를 지원할 수 있습니다.

1. **AWS 콘솔 → CloudFront 이동**
2. **[배포 생성] 클릭**
3. **기본 원본 도메인에 S3 버킷 URL 입력**
4. **[배포 생성]**

도메인 연결 시 `CNAME`을 추가하여 연결합니다.

## 8. 최종 정리

✔ **EC2에서 AWS CLI를 사용하여 S3로 정적 파일 업로드**
✔ **S3가 정적 파일을 제공하고, Express는 API 서버 역할만 수행**
✔ **Route 53을 이용하여 S3 웹사이트에 도메인 연결**
✔ **CloudFront를 활용하여 HTTPS 적용 가능**

이제 EC2에서 Express를 API 서버로 전환하고, S3를 통해 정적 웹사이트를 서빙하는 구조를 완성할 수 있습니다! 🚀

