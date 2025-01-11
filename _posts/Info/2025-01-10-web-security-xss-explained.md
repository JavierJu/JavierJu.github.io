---
title: "Cross-Site Scripting (XSS): 개념과 방어 방법"
excerpt: "Cross-Site Scripting(XSS)의 원리, 종류, 위험성, 그리고 효과적인 방어 방법에 대해 알아봅니다."
categories:
  - Web Security
tags:
  - [XSS, 웹 보안, 취약점, 클라이언트 보안]
permalink: /info/web-security-xss-explained/
toc: true
toc_sticky: true
date: 2025-01-10
last_modified_at: 2025-01-10
---

**Cross-Site Scripting (XSS)**는 웹 애플리케이션 보안 취약점 중 하나로, 공격자가 악성 스크립트를 웹 페이지에 삽입하여 다른 사용자에게 실행되도록 하는 공격입니다. 이를 통해 공격자는 피해자의 브라우저에서 악성 코드를 실행하거나, 세션 토큰, 쿠키, 또는 민감한 데이터를 탈취할 수 있습니다. XSS는 주로 웹 애플리케이션의 입력 검증 및 출력 처리 부족으로 발생합니다.

## XSS의 주요 유형

### 1. Stored XSS (Persistent XSS)
- 악성 스크립트가 서버에 영구적으로 저장되는 경우.
- **예**: 댓글, 게시글, 프로필 정보 등에 삽입된 스크립트가 다른 사용자가 해당 페이지를 열 때 실행.

```html
<!-- 댓글 입력에 삽입된 악성 스크립트 -->
<script>alert('Hacked!');</script>
```

### 2. Reflected XSS
- 악성 스크립트가 요청에 포함되어 서버로 전달되고, 응답 페이지에서 그대로 반환되는 경우.
- 주로 URL 파라미터나 폼 입력 데이터를 이용.

```html
<!-- 피해자가 클릭한 악성 링크 -->
http://example.com/search?q=<script>alert('Hacked!');</script>
```

### 3. DOM-based XSS
- 서버가 아닌 클라이언트 쪽 코드에서 발생.
- 자바스크립트가 DOM(Document Object Model)을 조작하는 과정에서 악성 코드가 실행됨.

```javascript
// DOM에서 사용자 입력을 처리하는 취약한 코드
const userInput = location.hash;
document.write(userInput);
```

## XSS의 위험성

- **데이터 탈취**: 사용자의 세션 쿠키, 로컬 스토리지 데이터를 훔침.
- **피싱**: 악성 웹 페이지를 통해 사용자로부터 민감 정보를 얻음.
- **브라우저 기반 악성 코드 실행**: 사용자의 브라우저에서 임의의 코드 실행.
- **서비스 방해**: 웹 애플리케이션의 정상 동작 방해.

## 방어 방법

### 1. 입력 데이터 검증 (Input Validation)
- 사용자가 입력한 데이터를 철저히 검증하고, 예상치 못한 입력을 차단.
- 허용된 문자만 허용하는 화이트리스트 접근법 권장.

### 2. 출력 데이터 인코딩 (Output Encoding)
- HTML, JavaScript, CSS 등으로 데이터를 출력할 때 반드시 인코딩 처리.

```javascript
// 예시: 출력 데이터 인코딩
const escapedInput = input.replace(/</g, "&lt;").replace(/>/g, "&gt;");
document.innerHTML = escapedInput;
```

### 3. Content Security Policy (CSP)
- 브라우저에서 실행 가능한 리소스를 제한하여 악성 스크립트 실행 방지.

```http
Content-Security-Policy: script-src 'self';
```

### 4. HTTPOnly 및 Secure 플래그 사용
- 쿠키에 `HttpOnly` 플래그를 설정하면 자바스크립트를 통해 쿠키를 접근할 수 없게 설정.
- `Secure` 플래그로 HTTPS 연결에서만 쿠키를 전송.

### 5. 라이브러리 활용
- 검증된 보안 라이브러리를 사용하여 XSS를 예방.
- 예: React와 같은 프레임워크는 기본적으로 출력값을 escape 처리.

### 6. 유저 입력 데이터 Sanitization
- HTML Purifier와 같은 라이브러리를 사용하여 유저 입력을 필터링.

## XSS 탐지 방법

### 1. 취약점 스캐너 사용
- OWASP ZAP, Burp Suite와 같은 도구로 취약점을 자동으로 탐지.

### 2. 수동 테스트
- 악의적 입력값을 사용해 수동으로 테스트.

### 3. 로그 및 모니터링
- 의심스러운 활동과 스크립트 실행을 지속적으로 모니터링.

---

XSS는 애플리케이션의 보안 취약점을 악용해 사용자에게 큰 피해를 줄 수 있는 공격입니다. 이를 방지하려면 입력 검증, 출력 처리, 정책 설정 등을 철저히 해야 합니다.
