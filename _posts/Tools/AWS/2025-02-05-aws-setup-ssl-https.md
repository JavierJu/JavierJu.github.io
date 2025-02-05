---
title: "AWS에서 SSL 인증서 설정하고 HTTPS 적용하는 방법"
excerpt: "AWS Certificate Manager(ACM)과 Application Load Balancer(ALB)를 사용하여 SSL 인증서를 설정하고 HTTPS를 적용하는 방법을 단계별로 설명합니다. Nginx나 Apache 없이 AWS 환경에서 HTTPS를 활성화하는 방법을 알아보세요."
categories:
  - AWS
  - DevOps
  - SSL
  - 보안
  
tags:
  - [AWS, SSL, HTTPS, ACM, ALB, 보안, 인증서]
permalink: /aws/setup-ssl-https/
toc: true
toc_sticky: true
date: 2025-02-05
last_modified_at: 2025-02-05
---

웹 애플리케이션을 운영할 때 HTTPS를 적용하면 보안이 강화되고 SEO에도 긍정적인 영향을 미칩니다. AWS에서는 **AWS Certificate Manager(ACM)**과 **Application Load Balancer(ALB)**를 사용하여 손쉽게 SSL 인증서를 설정할 수 있습니다. 이 글에서는 **Nginx나 Apache를 사용하지 않고** AWS 환경에서 HTTPS를 적용하는 방법을 설명합니다.

---

## 1. AWS Certificate Manager(ACM)에서 SSL 인증서 발급

AWS Certificate Manager(ACM)는 무료 SSL 인증서를 제공하며, 자동 갱신을 지원합니다. 다음 단계에 따라 SSL 인증서를 발급받습니다.

### 1.1 ACM 콘솔에서 인증서 요청

1. AWS 콘솔에서 `Certificate Manager`를 검색하여 이동합니다.
2. **"Provision certificates"** 클릭 후, **"Request a public certificate"**을 선택합니다.
3. **도메인 이름 입력**: `javierju.com` 및 `www.javierju.com`을 입력합니다.
4. **DNS 검증 방식 선택**: `DNS validation - recommended`를 선택합니다.

### 1.2 DNS 검증 레코드 추가

- AWS Route 53을 사용하고 있다면, `Create record in Route 53` 버튼을 클릭하여 자동으로 DNS 레코드를 추가할 수 있습니다.
- 다른 DNS 제공자를 사용하는 경우 ACM에서 제공하는 **CNAME 레코드**를 수동으로 추가해야 합니다.
- DNS 설정이 완료되면 ACM에서 인증서 상태가 `Issued`로 변경됩니다.

---

## 2. Application Load Balancer(ALB) 설정

HTTPS를 적용하려면 **Application Load Balancer(ALB)**를 사용하여 SSL 인증서를 연결해야 합니다.

### 2.1 ALB 생성

1. AWS 콘솔에서 `EC2 > Load Balancers`로 이동합니다.
2. **"Create Load Balancer"**를 클릭하고 **Application Load Balancer(ALB)**를 선택합니다.
3. **Listener 설정**에서 `HTTP(80)`과 `HTTPS(443)` 포트를 추가합니다.
4. **VPC 및 서브넷 선택**: 인스턴스가 위치한 서브넷을 선택합니다.

### 2.2 ACM 인증서 연결

1. HTTPS(443) Listener를 설정할 때, **"Add listener"**를 클릭합니다.
2. **"HTTPS"**를 선택한 후, `ACM에서 발급받은 SSL 인증서`를 연결합니다.
3. **보안 정책(Security policy)**을 선택하고 설정을 저장합니다.

### 2.3 Target Group 설정 (EC2 연결)

1. ALB가 요청을 전달할 **Target Group**을 생성합니다.
2. **Protocol**을 `HTTP`로 설정하고, EC2 인스턴스를 추가합니다.
3. **Health Check**를 설정하여 정상적인 EC2 인스턴스만 트래픽을 받을 수 있도록 합니다.

---

## 3. HTTP에서 HTTPS로 리디렉션 설정

ALB에서 HTTP 요청을 자동으로 HTTPS로 리디렉션하려면 **Listener 규칙을 설정**해야 합니다.

### 3.1 리디렉션 규칙 추가

1. AWS 콘솔에서 ALB의 **Listener** 설정으로 이동합니다.
2. `HTTP:80` Listener에서 **"View/edit rules"**를 클릭합니다.
3. 새로운 규칙을 추가하여 `HTTP 요청을 HTTPS로 리디렉션`합니다.

### 3.2 HTTP → HTTPS 리디렉션 규칙 예시

```json
{
  "Conditions": [],
  "Actions": [
    {
      "Type": "redirect",
      "RedirectConfig": {
        "Protocol": "HTTPS",
        "Port": "443",
        "StatusCode": "HTTP_301"
      }
    }
  ]
}
```

이제 HTTP(`http://javierju.com`)로 접속하면 자동으로 HTTPS(`https://javierju.com`)로 리디렉션됩니다.

---

## 4. EC2 보안 그룹 설정

ALB가 EC2 인스턴스와 통신할 수 있도록 보안 그룹을 설정해야 합니다.

### 4.1 보안 그룹 규칙 설정

EC2 인스턴스의 보안 그룹에서 다음 규칙을 추가합니다:

- **포트 80 (HTTP)**: ALB에서 EC2로 요청을 전달하기 위해 허용
- **포트 443 (HTTPS)**: HTTPS를 직접 사용할 경우 필요
- **ALB의 보안 그룹에서 EC2의 보안 그룹을 허용하도록 설정**

```bash
# HTTP 허용
aws ec2 authorize-security-group-ingress --group-id sg-12345678 --protocol tcp --port 80 --source-group sg-alb

# HTTPS 허용
aws ec2 authorize-security-group-ingress --group-id sg-12345678 --protocol tcp --port 443 --source-group sg-alb
```

---

## 5. SSL 인증서 자동 갱신 확인

ACM 인증서는 자동으로 갱신되지만, 정상적으로 동작하는지 확인하려면 다음 명령어를 실행할 수 있습니다.

```bash
aws acm list-certificates
```

또한 AWS에서 자동으로 인증서를 갱신할 수 있도록 설정되어 있는지 확인하세요.

---

## 결론

AWS에서 SSL 인증서를 적용하고 HTTPS를 활성화하는 방법을 정리하면 다음과 같습니다:

1. **AWS Certificate Manager(ACM)**에서 SSL 인증서를 발급받고 DNS 검증을 완료합니다.
2. **Application Load Balancer(ALB)**를 생성하고 HTTPS(443) Listener를 설정합니다.
3. **ACM 인증서를 ALB에 연결**하고 EC2 Target Group을 설정합니다.
4. **HTTP → HTTPS 자동 리디렉션을 설정**하여 모든 트래픽이 HTTPS로 이동하도록 합니다.
5. **EC2 보안 그룹을 설정**하여 ALB에서 EC2로 요청이 정상적으로 전달되도록 합니다.
6. **SSL 인증서 자동 갱신**이 설정되어 있는지 확인합니다.

이 방법을 사용하면 **Nginx나 Apache 없이도** AWS 환경에서 HTTPS를 적용할 수 있습니다. 추가적인 설정이나 질문이 있다면 댓글로 남겨주세요!

