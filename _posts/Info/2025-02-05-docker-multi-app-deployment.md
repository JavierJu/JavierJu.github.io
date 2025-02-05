---
title: "Docker를 활용한 멀티 애플리케이션 배포 방법"
excerpt: "Docker를 활용하여 여러 개의 애플리케이션을 독립적인 컨테이너로 실행하고 관리하는 방법을 설명합니다. Docker Compose, Nginx 리버스 프록시 설정 및 네트워크 구성에 대한 상세한 코드 예제와 함께 배포 전략을 다룹니다."
categories:
  - DevOps
  - Docker
  - Deployment
  - Backend
  - info
  
tags:
  - [Docker, DevOps, 컨테이너, 배포, Nginx, 리버스 프록시]
permalink: /docker/multi-app-deployment/
toc: true
toc_sticky: true
date: 2025-02-05
last_modified_at: 2025-02-05
---

# Docker를 활용한 멀티 애플리케이션 배포 방법

## 1. Docker란?

Docker는 애플리케이션을 독립적인 컨테이너로 실행하는 기술로, 각 애플리케이션을 다른 애플리케이션과 격리하여 실행할 수 있도록 도와줍니다. 이를 통해 같은 포트를 사용하는 여러 개의 애플리케이션을 실행할 때 발생할 수 있는 충돌 문제를 방지할 수 있습니다.

## 2. Docker의 핵심 개념

### 2.1. 컨테이너(Container)
컨테이너는 애플리케이션과 필요한 모든 라이브러리를 포함하는 독립적인 실행 환경입니다.

### 2.2. 이미지(Image)
이미지는 컨테이너를 실행하기 위한 템플릿으로, 애플리케이션 코드 및 실행 환경을 포함합니다.

### 2.3. Dockerfile
Dockerfile은 이미지를 생성하기 위한 설정 파일로, 애플리케이션을 컨테이너화하는 방법을 정의합니다.

### 2.4. Docker Compose
Docker Compose는 여러 개의 컨테이너를 관리하는 툴로, `docker-compose.yml` 파일을 사용하여 애플리케이션을 정의합니다.

## 3. Docker를 활용한 멀티 애플리케이션 실행

### 3.1. Dockerfile 작성
각 애플리케이션을 Docker 컨테이너로 실행하려면 `Dockerfile`을 작성해야 합니다. 예제는 Node.js 애플리케이션을 위한 설정입니다.

```dockerfile
# Node.js 공식 이미지 사용
FROM node:16

# 작업 디렉토리 설정
WORKDIR /app

# 의존성 파일 복사
COPY package*.json ./

# 의존성 설치
RUN npm install

# 애플리케이션 코드 복사
COPY . .

# 애플리케이션 실행 명령어
CMD ["npm", "start"]
```

### 3.2. Docker Compose 설정
여러 개의 애플리케이션을 실행할 때는 `docker-compose.yml`을 사용합니다.

```yaml
version: '3'

services:
  app1:
    build:
      context: ./app1
    ports:
      - "3000:3000"
  
  app2:
    build:
      context: ./app2
    ports:
      - "4000:4000"
```

위 설정에서 `app1`과 `app2`는 서로 다른 폴더에서 빌드되며, 각각 3000번과 4000번 포트에서 실행됩니다.

### 3.3. Nginx 리버스 프록시 설정
외부에서 애플리케이션에 접근할 수 있도록 Nginx를 리버스 프록시로 설정할 수 있습니다.

```nginx
server {
    listen 80;

    location /app1 {
        proxy_pass http://app1:3000;
    }

    location /app2 {
        proxy_pass http://app2:4000;
    }
}
```

### 3.4. Docker 네트워크 구성
Docker 컨테이너 간의 통신을 원활하게 하려면 네트워크를 설정해야 합니다.

```bash
docker network create mynetwork
docker run --network=mynetwork --name app1 app1-image
docker run --network=mynetwork --name app2 app2-image
```

## 4. 결론
Docker를 활용하면 여러 개의 애플리케이션을 독립적인 컨테이너로 실행하여 포트 충돌 없이 배포할 수 있습니다. 또한, Docker Compose와 Nginx 리버스 프록시를 사용하면 보다 효율적으로 애플리케이션을 관리할 수 있습니다.

