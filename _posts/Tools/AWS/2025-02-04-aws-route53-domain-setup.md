---
title: "AWS Route 53으로 도메인 구매 및 설정하는 방법"
excerpt: "Amazon Route 53을 이용해 도메인을 구매하고, DNS 설정을 통해 웹 애플리케이션을 배포하는 방법을 설명합니다. EC2와의 연결 방법도 코드 예제와 함께 다룹니다."
categories:
  - AWS
  - DevOps
  - Cloud
  - Web Hosting
  
tags:
  - [AWS, Route 53, 도메인, DNS, EC2, 클라우드]
permalink: /aws/route53-domain-setup/
toc: true
toc_sticky: true
date: 2025-02-04
last_modified_at: 2025-02-04
---

## Amazon Route 53 개요

Amazon Route 53은 AWS에서 제공하는 DNS(Domain Name System) 서비스로, 도메인 이름을 등록하고, DNS 레코드를 설정하여 인터넷에서 애플리케이션을 사용할 수 있도록 해줍니다. 도메인 구입부터 EC2와 연결하는 과정까지 자세히 설명하겠습니다.

---

## 1. Route 53에서 도메인 구매하기

1. AWS 콘솔에서 **Route 53** 서비스로 이동합니다.
2. **"Domain registration"** 메뉴로 이동합니다.
3. **"Register Domain"**을 클릭하여 원하는 도메인 이름을 검색합니다.
4. 사용 가능한 도메인을 선택한 후, 개인 정보 및 결제 정보를 입력하고 구매를 완료합니다.

---

## 2. DNS 설정 (Hosted Zone 생성 및 레코드 추가)

도메인을 구매한 후, 해당 도메인의 DNS 설정을 관리하기 위해 Hosted Zone을 생성합니다.

### 2.1 Hosted Zone 생성

1. AWS Route 53 콘솔에서 **"Hosted Zones"** 메뉴로 이동합니다.
2. **"Create hosted zone"** 버튼을 클릭합니다.
3. 도메인 이름을 입력하고, *Public hosted zone*을 선택한 후 생성합니다.

### 2.2 A 레코드 추가 (EC2 인스턴스 연결)

도메인을 AWS의 EC2 인스턴스와 연결하려면 A 레코드를 추가해야 합니다.

1. Hosted Zone에서 **"Create Record"**를 클릭합니다.
2. **Record name**: 공란으로 두거나 `www`와 같은 서브도메인을 입력합니다.
3. **Record type**: `A` 선택
4. **Value**: EC2 퍼블릭 IP 입력
5. **TTL**: 기본값(300초) 유지
6. **Create records** 버튼 클릭

#### 예제 (Terraform을 사용한 Route 53 A 레코드 설정)
```hcl
resource "aws_route53_record" "my_domain" {
  zone_id = aws_route53_zone.my_zone.zone_id
  name    = "example.com"
  type    = "A"
  ttl     = 300
  records = ["12.34.56.78"]  # EC2 퍼블릭 IP
}
```

---

## 3. 도메인과 애플리케이션 연결 테스트

DNS 레코드 설정 후, 브라우저에서 도메인 (`example.com`)을 입력하여 웹 애플리케이션이 정상적으로 연결되는지 확인합니다. DNS 변경 사항이 반영되는 데 몇 분에서 몇 시간이 걸릴 수 있습니다.

### nslookup으로 도메인 확인
```sh
nslookup example.com
```

### curl로 연결 테스트
```sh
curl -I http://example.com
```

---

## 결론
Amazon Route 53을 사용하여 도메인을 등록하고 DNS 설정을 통해 EC2 인스턴스와 연결하는 방법을 알아보았습니다. 이를 통해 AWS에서 웹 애플리케이션을 배포하고, 도메인을 통해 접근할 수 있도록 설정할 수 있습니다.

