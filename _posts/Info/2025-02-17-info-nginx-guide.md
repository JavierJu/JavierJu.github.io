---
title: "Nginx란? 개념부터 실전 사용법까지 정리"
excerpt: "Nginx의 개념, 주요 기능, Apache와의 차이점, 리버스 프록시, 로드 밸런싱, 정적 파일 제공, HTTPS 설정 등 실전 사용법을 코드 예제와 함께 설명합니다."
categories:
  - DevOps
  - Web Server
  - Backend
  - Networking
  - Info

tags:
  - [Nginx, Web Server, Reverse Proxy, Load Balancing, HTTPS]
permalink: /info/nginx-guide/
toc: true
toc_sticky: true
date: 2025-02-17
last_modified_at: 2025-02-17
---

**Nginx(엔진엑스)**는 고성능 웹 서버이자 리버스 프록시, 로드 밸런서, 캐시 서버 등의 역할을 수행하는 오픈 소스 소프트웨어입니다. 초기에는 HTTP 서버로 개발되었으나, 현재는 다양한 네트워크 프로토콜을 처리할 수 있도록 확장되었습니다.

## 1. Nginx의 주요 기능

### 1) 웹 서버(Web Server)

- 정적 파일(HTML, CSS, JS, 이미지 등) 제공에 최적화됨
- 높은 동시 접속 처리 성능 제공

### 2) 리버스 프록시(Reverse Proxy)

- 클라이언트 요청을 실제 애플리케이션 서버(Node.js, Django 등)로 전달
- 보안 강화 및 SSL 종료(TLS Offloading) 기능 제공

### 3) 로드 밸런서(Load Balancer)

- 여러 애플리케이션 서버로 요청을 분산하여 성능 최적화

### 4) 캐시 서버(Cache Server)

- 정적 콘텐츠 및 응답을 캐싱하여 성능 향상

### 5) HTTP/HTTPS 처리

- SSL/TLS 암호화를 통한 보안 강화 가능

### 6) WebSocket 및 gRPC 지원

- 실시간 데이터 전송을 위한 WebSocket 및 gRPC 트래픽 처리 가능

---

## 2. Nginx vs. Apache

| 특징           | Nginx                                | Apache                      |
| -------------- | ------------------------------------ | --------------------------- |
| 처리 방식      | 비동기 이벤트 기반                   | 멀티스레드 기반             |
| 성능           | 높은 동시 접속 처리                  | 요청 증가 시 성능 저하 가능 |
| 정적 파일 처리 | 빠름                                 | 상대적으로 느림             |
| 설정 파일      | 직관적 (`nginx.conf`)                | 모듈화된 설정 (`.htaccess`) |
| 확장성         | 리버스 프록시, 로드 밸런싱 기본 지원 | 모듈을 통해 확장            |
| 메모리 사용량  | 낮음                                 | 상대적으로 높음             |

---

## 3. Nginx 기본 사용법

### 1) Nginx 설치 (Ubuntu 기준)

```bash
sudo apt update
sudo apt install nginx -y
```

### 2) Nginx 서비스 관리

```bash
sudo systemctl start nginx   # 시작
sudo systemctl stop nginx    # 중지
sudo systemctl restart nginx # 재시작
sudo systemctl status nginx  # 상태 확인
```

### 3) 기본 설정 파일 (`nginx.conf`)

```nginx
worker_processes auto;

events {
    worker_connections 1024;
}

http {
    server {
        listen 80;
        server_name example.com;

        location / {
            root /var/www/html;
            index index.html;
        }
    }
}
```

---

## 4. Nginx 리버스 프록시 설정 (MERN 스택 예제)

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://localhost:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

```bash
sudo nginx -t  # 설정 확인
sudo systemctl restart nginx  # Nginx 재시작
```

---

## 5. HTTPS 설정 (Let's Encrypt 무료 SSL)

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d example.com -d www.example.com
```

SSL 인증서를 자동 갱신하도록 설정:

```bash
sudo certbot renew --dry-run
```

---

## 6. Nginx 로드 밸런싱 설정

```nginx
upstream backend_servers {
    server 192.168.1.10:5000;
    server 192.168.1.11:5000;
    server 192.168.1.12:5000;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend_servers;
        proxy_set_header Host $host;
    }
}
```

---

## 7. 정적 파일 제공 설정

```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/html;
    index index.html;

    location /images/ {
        root /var/www/static/;
    }
}
```

---

## 8. Nginx 로깅 설정

### 로그 파일 위치

- **접속 로그:** `/var/log/nginx/access.log`
- **에러 로그:** `/var/log/nginx/error.log`

### 로그 포맷 변경 (`nginx.conf`)

```nginx
log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                '$status $body_bytes_sent "$http_referer" '
                '"$http_user_agent" "$http_x_forwarded_for"';

access_log /var/log/nginx/access.log main;
```

---

## 9. 결론

Nginx는 가볍고 성능이 뛰어난 웹 서버로, **MERN 스택, Express, Next.js, React 등과 함께 배포**할 때 유용합니다.

🚀 **배포 시 Nginx를 활용하면 더 안정적이고 효율적인 서버 구성을 할 수 있습니다!**
