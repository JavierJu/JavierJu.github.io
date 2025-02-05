---
title: "AWS WAF 설정 최적화: 보안 강화를 위한 Managed Rules 적용 가이드"
excerpt: "AWS WAF의 무료 Managed Rules를 활용하여 웹 애플리케이션 보안을 강화하는 방법을 설명합니다. SQL Injection, XSS, 악성 IP 차단 등의 보호 기능을 활성화하고, Rate-based Rule을 설정하는 방법을 코드 예제와 함께 소개합니다."
categories:
  - AWS
  - Security
  - Cloud
  - DevOps
  - Web Application
  
tags:
  - [AWS, WAF, Security, Web Security, SQL Injection, XSS, DDoS]
permalink: /aws/waf-security-rules/
toc: true
toc_sticky: true
date: 2025-02-05
last_modified_at: 2025-02-05
---

AWS WAF(Web Application Firewall)는 웹 애플리케이션을 보호하는 강력한 보안 솔루션입니다. 본 포스트에서는 **AWS에서 무료로 제공하는 Managed Rules**를 활용하여 **SQL Injection, XSS, DDoS 등으로부터 웹 애플리케이션을 보호하는 방법**을 소개합니다.

## 1. AWS WAF 기본 개념
AWS WAF는 HTTP/S 트래픽을 검사하고 설정된 규칙에 따라 악의적인 요청을 차단하는 역할을 합니다. WAF는 **CloudFront, ALB(Application Load Balancer), API Gateway** 등에 적용할 수 있습니다.

### 주요 기능
- **SQL Injection, XSS 등의 웹 공격 방어**
- **DDoS 및 과도한 요청 방어** (Rate-based Rule 활용)
- **악성 IP 및 봇 차단**
- **관리자 페이지 보호** (`/admin`, `/wp-admin` 등 차단 가능)


## 2. AWS WAF Managed Rules 적용하기

### 현재 적용된 AWS WAF Rules 목록
AWS WAF의 무료 Managed Rules를 활용하여 보안을 강화할 수 있습니다. 아래는 **무료로 적용 가능한 주요 Rules** 목록입니다.

```plaintext
AWS-AWSManagedRulesAmazonIpReputationList    → 악성 IP 차단 (스팸, 봇, VPN 등)
AWS-AWSManagedRulesCommonRuleSet             → 일반적인 웹 공격 방어 (SQLi, XSS 등)
AWS-AWSManagedRulesKnownBadInputsRuleSet     → 악성 입력 감지 및 차단
AWS-AWSManagedRulesSQLiRuleSet               → SQL Injection 공격 방어 강화
AWS-AWSManagedRulesAnonymousIpList           → 익명 프록시, VPN, Tor 네트워크 차단
AWS-AWSManagedRulesWindowsRuleSet            → Windows 기반 공격 방어
AWS-AWSManagedRulesAdminProtectionRuleSet    → 관리자 페이지 보호
```

위 규칙들을 적용하면 기본적인 웹 보안이 강화되며, AWS에서 지속적으로 업데이트되는 보안 정책을 활용할 수 있습니다.


## 3. Rate-based Rule 추가 (DDoS 방어 및 과도한 요청 차단)

Rate-based Rule을 설정하면 **일정 시간 동안 특정 IP에서 과도한 요청이 발생하면 차단**할 수 있습니다. 예를 들어, **5분 동안 1000개 이상의 요청을 보내는 IP를 차단**하는 설정을 적용할 수 있습니다.

### 📌 **Rate-based Rule 생성 방법**
1. **AWS WAF 콘솔 접속** → Web ACL 선택
2. **Rules 탭**에서 "Add Rule" 클릭
3. "Rule Type"에서 "Rate-based Rule" 선택
4. **설정 값 입력:**
   - **Rate limit:** `1000 requests per 5 minutes`
   - **Action:** `Block`
   
```json
{
  "Name": "RateLimitRule",
  "Priority": 10,
  "Action": {
    "Block": {}
  },
  "Statement": {
    "RateBasedStatement": {
      "Limit": 1000,
      "AggregateKeyType": "IP"
    }
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "RateLimitRule"
  }
}
```

이제 5분 동안 1000개 이상의 요청을 보내는 IP는 **자동으로 차단**됩니다.


## 4. WAF Rules의 Action 확인 및 "Block" 설정

기본적으로 AWS Managed Rules는 "Use rule actions" 상태로 적용되는데, 일부 Rule은 **Count 모드로 설정될 수도 있습니다.**

### 📌 **각 Rule이 Count 모드인지 확인하고 Block으로 변경하는 방법**
1. **AWS WAF → Web ACL → Rules** 로 이동
2. 각 Rule을 클릭하여 설정 확인
3. **Action이 "Count"이면 "Block"으로 변경**
4. 변경 후 **저장**

```json
{
  "Name": "SQLiRuleSet",
  "Priority": 5,
  "Action": {
    "Block": {}
  }
}
```

이렇게 설정하면 SQL Injection이 탐지될 경우 **자동으로 차단(Block)** 됩니다.


## 5. AWS WAF 로그 모니터링 및 테스트

설정이 정상적으로 적용되었는지 확인하려면 **AWS WAF 로그 및 CloudFront 로그를 확인하는 것이 중요합니다.**

### 📌 **로그 모니터링 활성화 방법**
1. **AWS WAF → Web ACL → Logging & Metrics**로 이동
2. **로그 활성화** (CloudWatch, S3, Kinesis Firehose 중 선택 가능)
3. CloudFront Access Logs도 활성화하여 **실제 차단된 요청을 분석**

### 📌 **테스트 방법**
- SQL Injection 공격 시도 (`' OR 1=1 --` 같은 페이로드 입력)
- XSS 공격 테스트 (`<script>alert('XSS')</script>` 입력)
- 동일 IP에서 짧은 시간 내에 과도한 요청을 보내고 차단되는지 확인


## 🚀 결론: 현재 설정 평가 및 추가 조치
✅ **무료 Managed Rules를 활용하여 웹 애플리케이션 보안을 최적화**
✅ **SQL Injection, XSS, 관리자 페이지 보호, 악성 IP 차단 등 필수 보안 적용**
✅ **Rate-based Rule 설정으로 과도한 요청 차단 가능**

⚡ **추가로 검토하면 좋은 설정:**
1. **GeoMatch Rule (특정 국가 IP 차단, 유료 기능)**
2. **더 세부적인 WAF 정책 추가 (예: 사용자 정의 IP 차단 리스트)**
3. **정기적인 로그 모니터링 및 테스트 진행**

이제 AWS WAF를 활용하여 **강력한 보안 환경을 구축**할 수 있습니다! 🚀

