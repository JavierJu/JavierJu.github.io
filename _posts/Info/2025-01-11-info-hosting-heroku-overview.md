---
title: "Heroku의 개요와 특징: 클라우드 앱 호스팅의 간편함"
excerpt: "Heroku의 주요 특징, 장점, 단점, 그리고 사용 방법을 살펴보며, 클라우드 기반 애플리케이션 호스팅에 적합한 PaaS 서비스를 이해합니다."
categories:
  - Hosting
  - Cloud
  - DevOps
tags:
  - [Heroku, PaaS, Cloud Hosting, App Deployment]
permalink: /info/hosting-heroku-overview/
toc: true
toc_sticky: true
date: 2025-01-11
last_modified_at: 2025-01-11
---

## Heroku란?

Heroku는 클라우드 기반의 PaaS(Platform as a Service) 호스팅 서비스로, 개발자가 서버 설정이나 유지 관리 없이 애플리케이션을 배포, 관리, 확장할 수 있도록 돕습니다. 특히 초기 개발 단계에서 빠르게 프로토타입을 제작하거나 애플리케이션을 배포하기에 적합합니다.

---

## 주요 특징

### 1. 간편한 배포와 설정
- Heroku는 Git 기반 배포를 지원하여, 간단한 명령어 하나로 애플리케이션을 배포할 수 있습니다.

```bash
# Heroku에 배포
$ git push heroku main
```

### 2. 다양한 언어 지원
- Node.js, Python, Ruby, Java, PHP, Scala, Go 등 여러 프로그래밍 언어를 지원합니다.

### 3. 애드온(Add-ons)
- PostgreSQL, Redis, RabbitMQ 등 다양한 서드파티 애드온을 추가해 애플리케이션 기능을 확장할 수 있습니다.

### 4. 자동 확장성
- 사용량 증가 시 Dyno(작업 단위)를 추가하여 애플리케이션을 자동으로 확장할 수 있습니다.

### 5. 환경 변수 관리
- 환경 변수를 대시보드나 CLI를 통해 간단히 관리할 수 있습니다.

```bash
# 환경 변수 설정
$ heroku config:set KEY=VALUE
```

---

## 사용 방법

### 1. Heroku CLI 설치
Heroku CLI를 설치하면 명령줄에서 계정을 설정하고 앱을 관리할 수 있습니다. [Heroku CLI 설치 링크](https://devcenter.heroku.com/articles/heroku-cli)

### 2. 계정 생성 및 로그인
[Heroku 공식 웹사이트](https://www.heroku.com/)에서 계정을 생성한 후 CLI로 로그인합니다.

```bash
# Heroku CLI 로그인
$ heroku login
```

### 3. 앱 생성 및 배포
로컬 프로젝트를 초기화하고 Heroku 앱을 생성한 후 배포합니다.

```bash
# Heroku 앱 생성
$ heroku create [앱 이름]

# Git 리포지토리 초기화 및 Heroku에 배포
$ git init
$ git add .
$ git commit -m "Initial commit"
$ git push heroku main
```

### 4. 데이터베이스 애드온 추가
Heroku에서 PostgreSQL 데이터베이스를 추가하는 예:

```bash
# PostgreSQL 애드온 추가
$ heroku addons:create heroku-postgresql:hobby-dev
```

### 5. 애플리케이션 실행
배포된 애플리케이션은 Heroku에서 제공한 URL로 접근 가능합니다.

---

## 장점

1. **초보자 친화적**: 간단한 설정과 직관적인 배포 과정.
2. **빠른 프로토타이핑**: 초기 개발 단계에서 신속한 앱 배포.
3. **강력한 생태계**: 다양한 애드온과 통합된 서비스.

---

## 단점

1. **무료 플랜 제한**: 무료 플랜에서는 Dyno가 슬립 상태로 전환될 수 있어 초기 로드 시간이 느림.
2. **비용 증가**: 사용량이 많아지거나 고급 기능이 필요할 경우 비용이 빠르게 증가.
3. **고급 설정 부족**: AWS나 GCP 같은 IaaS 플랫폼에 비해 세밀한 설정이 어려움.

---

Heroku는 개인 프로젝트, MVP(Minimum Viable Product), 학습용 프로젝트에 적합합니다. 간편한 배포와 관리가 강점이지만, 대규모 프로젝트에서는 AWS, GCP, Azure 같은 IaaS 플랫폼으로의 전환을 고려할 수 있습니다.

