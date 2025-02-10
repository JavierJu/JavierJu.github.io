---
title: "DNS 변경으로 사이트 접속 문제 해결하기: Google DNS & Cloudflare DNS 설정 가이드"
excerpt: "DNS 설정 문제로 인해 웹사이트에 접속할 수 없는 경우 Google DNS 또는 Cloudflare DNS로 변경하면 해결할 수 있습니다. 이번 포스팅에서는 DNS 개념과 변경 방법을 자세히 설명합니다."
categories:
  - DNS
  - 네트워크
  - 웹 개발
  - AWS
  - CloudFront
  
tags:
  - [DNS, 네트워크, Google DNS, Cloudflare DNS, 사이트 접속 오류, AWS, CloudFront, Route 53]
permalink: /aws/network-dns-change-google-cloudflare/
toc: true
toc_sticky: true
date: 2025-02-10
last_modified_at: 2025-02-10
---

웹사이트를 AWS S3와 CloudFront를 이용해 배포한 후, 특정 네트워크 환경에서 **도메인에 접속할 수 없는 문제**가 발생할 수 있습니다. 이 문제는 대부분 **DNS 설정 문제**에서 비롯되며, Google DNS 또는 Cloudflare DNS로 변경하면 해결할 수 있습니다.

이번 포스팅에서는 **DNS란 무엇이며, 왜 Google DNS & Cloudflare DNS를 사용해야 하는지, 그리고 설정 방법**을 코드 예제와 함께 설명합니다.

---

## ✅ DNS란 무엇인가?
**DNS(Domain Name System)** 는 우리가 입력하는 **도메인 이름(www.example.com)** 을 실제 웹 서버의 **IP 주소(192.168.x.x 또는 18.173.219.125 같은 숫자)** 로 변환해주는 시스템입니다.

💡 **비유하자면?**
- DNS는 인터넷의 **"전화번호부"** 같은 역할을 합니다.
- `www.google.com` → `142.250.190.78`
- `www.example.com` → `18.173.219.125`

즉, 도메인 주소를 사람이 이해하기 쉬운 형태로 유지하면서, 컴퓨터는 이를 숫자로 변환해 웹사이트에 접속할 수 있게 합니다.

---

## ❌ DNS 문제로 인해 발생하는 사이트 접속 오류
웹사이트를 배포한 후 아래와 같은 오류 메시지가 발생할 수 있습니다.

```
사이트에 연결할 수 없음
www.example.com의 IP 주소를 찾을 수 없습니다.
DNS_PROBE_FINISHED_NXDOMAIN
```

또는 **nslookup 명령어**를 실행했을 때 아래와 같은 오류가 나타날 수도 있습니다.

```sh
$ nslookup www.example.com
서버:    UnKnown
Address:  192.168.0.1

*** UnKnown이(가) www.example.com을(를) 찾을 수 없습니다. Non-existent domain
```

**이 문제는 대부분 ISP(인터넷 서비스 제공업체)의 DNS가 변경 사항을 반영하지 않아서 발생합니다.**

---

## ✅ Google DNS & Cloudflare DNS 변경의 필요성
기본적으로 인터넷 서비스 제공업체(ISP)에서 제공하는 DNS는 **업데이트가 느리고, 속도가 느리며, 보안이 취약**할 수 있습니다. 따라서 Google DNS 또는 Cloudflare DNS를 사용하면 아래와 같은 장점이 있습니다.

### 🔥 **Google & Cloudflare DNS의 장점**
✅ **빠른 도메인 응답 속도**: ISP DNS보다 빠르게 요청을 처리하여 웹사이트 접속 속도가 향상됩니다.  
✅ **최신 DNS 정보 반영**: 변경된 DNS 정보를 즉시 반영하여 사이트 접속 문제를 해결합니다.  
✅ **보안 강화**: 피싱 사이트 차단, 악성코드 방지 기능 제공.  
✅ **검열 우회 가능**: 일부 국가 또는 ISP가 차단한 사이트에도 접속할 수 있습니다.  

---

## 🛠️ Google DNS & Cloudflare DNS 설정 방법 (Windows 기준)

1. **제어판 → 네트워크 및 인터넷 → 네트워크 및 공유 센터 → 어댑터 설정 변경**
2. **사용 중인 네트워크(이더넷 또는 Wi-Fi) → 속성 클릭**
3. **"인터넷 프로토콜 버전 4(TCP/IPv4)" 선택 → "속성" 클릭**
4. **아래와 같이 DNS 주소를 변경**

### 🔹 **Google DNS 설정**
```
기본 설정 DNS 서버: 8.8.8.8
보조 DNS 서버: 8.8.4.4
```

### 🔹 **Cloudflare DNS 설정**
```
기본 설정 DNS 서버: 1.1.1.1
보조 DNS 서버: 1.0.0.1
```

5. **확인 후 적용!**
6. **명령 프롬프트에서 DNS 캐시 초기화 실행**
```sh
ipconfig /flushdns
```

---

## 🏁 결론: 이번 문제 해결 과정 요약

1. **도메인 A 레코드를 CloudFront로 설정했지만, ISP의 DNS가 이를 반영하지 않아서 `NXDOMAIN` 오류 발생**
2. **기본 ISP DNS 대신 Google DNS 또는 Cloudflare DNS로 변경 후 해결**
3. **변경된 DNS 서버는 최신 정보를 빠르게 반영하므로 사이트 정상 접속 가능!** 🎉  

**앞으로도 DNS 문제 발생 시 Google DNS 또는 Cloudflare DNS 변경을 고려하면 빠르게 해결할 수 있습니다!** 🚀

