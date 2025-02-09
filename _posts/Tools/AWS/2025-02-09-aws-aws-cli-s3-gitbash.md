---
title: "Windows에서 AWS CLI를 사용하여 Git Bash에서 S3 파일 업로드하기"
excerpt: "AWS CLI를 Windows에 설치하고 Git Bash(VS Code)에서 직접 S3로 데이터를 전송하는 방법을 단계별로 설명합니다. AWS 인증 설정부터 파일 업로드, 다운로드까지 코드 예제와 함께 알아봅니다."
categories:
  - AWS
  - DevOps
  - Cloud
  - Git
  - CLI
  
tags:
  - [AWS, S3, Git Bash, CLI, Windows, Cloud]
permalink: /aws/aws-cli-s3-gitbash/
toc: true
toc_sticky: true
date: 2025-02-09
last_modified_at: 2025-02-09
---

AWS CLI를 활용하면 로컬 환경에서 AWS 리소스를 쉽게 관리할 수 있습니다. 이 글에서는 **Windows에 AWS CLI를 설치하고, Git Bash(VS Code)에서 S3 버킷으로 파일을 업로드하는 방법**을 단계별로 설명합니다.

---

## **1. AWS CLI 설치 및 설정**

### **(1) AWS CLI 설치**

1. [AWS CLI 공식 다운로드 페이지](https://aws.amazon.com/cli/)로 이동합니다.
2. Windows 용 **"AWS CLI 설치 프로그램"**을 다운로드합니다.
3. 다운로드한 `.msi` 파일을 실행하여 설치를 완료합니다.

설치가 완료되면 **Git Bash 또는 명령 프롬프트(cmd)**에서 다음 명령어로 확인할 수 있습니다:

```sh
aws --version
```

출력 예시:

```sh
aws-cli/2.x.x Windows/10 ...
```

---

### **(2) AWS CLI 인증 정보 설정**

AWS 계정의 **Access Key**와 **Secret Key**를 설정해야 합니다.

#### 1. AWS IAM에서 사용자 생성
- [AWS IAM 콘솔](https://console.aws.amazon.com/iam/)로 이동합니다.
- `사용자(User)`를 생성하고, **AmazonS3FullAccess** 권한을 부여합니다.
- 생성된 `액세스 키(Access Key)`와 `비밀 키(Secret Key)`를 복사해둡니다.

#### 2. AWS CLI에서 인증 정보 입력

Git Bash를 실행하고 다음 명령어를 입력합니다:

```sh
aws configure
```

입력 예시:

```
AWS Access Key ID [None]: AKIAXXXXXXXX
AWS Secret Access Key [None]: XXXXXXXXXXXX
Default region name [None]: ap-northeast-1  # 도쿄 리전 (원하는 리전 입력)
Default output format [None]: json  # json, table, text 중 선택 가능
```

설정 파일은 `C:\Users\Javier\.aws\credentials`에 저장됩니다.

설정 확인:
```sh
cat ~/.aws/credentials
```

---

## **2. S3 버킷 생성 (필요한 경우)**

Git Bash에서 AWS CLI 명령어를 사용하여 S3 버킷을 생성할 수 있습니다.

```sh
aws s3 mb s3://my-portfolio-bucket --region ap-northeast-1
```

> `my-portfolio-bucket` → 원하는 버킷 이름으로 변경 (전 세계에서 유일해야 함)

버킷 리스트 확인:
```sh
aws s3 ls
```

---

## **3. Git Bash에서 S3로 파일 업로드**

### **(1) 단일 파일 업로드**
```sh
aws s3 cp ./myfile.txt s3://my-portfolio-bucket/
```

### **(2) 폴더(디렉토리) 업로드**
```sh
aws s3 cp ./myfolder s3://my-portfolio-bucket/ --recursive
```

### **(3) 업로드한 파일 리스트 확인**
```sh
aws s3 ls s3://my-portfolio-bucket/
```

---

## **4. Git Bash에서 S3 파일 다운로드**
```sh
aws s3 cp s3://my-portfolio-bucket/myfile.txt ./local-folder/
```

---

## **5. Git Bash에서 S3 파일 삭제**
```sh
aws s3 rm s3://my-portfolio-bucket/myfile.txt
```

---

## ✅ **추가 팁: S3 정적 웹사이트 호스팅 설정**

S3를 정적 웹사이트로 호스팅하려면 다음과 같이 설정할 수 있습니다.

```sh
aws s3 website s3://my-portfolio-bucket/ --index-document index.html
```

퍼블릭 읽기 권한을 추가하려면 **bucket policy**를 설정해야 합니다.

```sh
aws s3api put-bucket-policy --bucket my-portfolio-bucket --policy file://policy.json
```

`policy.json` 파일 내용 예시:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-portfolio-bucket/*"
    }
  ]
}
```

---

이제 AWS CLI를 활용하여 Git Bash에서 직접 S3와 연동할 수 있습니다! 🚀  
궁금한 점이 있다면 댓글로 남겨주세요. 😊

