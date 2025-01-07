---
title: "NPM dotenv: Node.js 환경 변수 관리의 완벽 가이드"
excerpt: "Node.js 환경 변수 관리 라이브러리인 dotenv의 설치, 사용법, 주요 기능, 그리고 환경 변수 관리 팁을 알아봅니다."
categories:
  - Node
  - Environment
  - Guide
tags:
  - [Node.js, dotenv, 환경 변수, Backend, JavaScript]
permalink: /node/npm-dotenv-guide/
toc: true
toc_sticky: true
date: 2025-01-07
last_modified_at: 2025-01-07
---

## NPM `dotenv` 모듈이란?

`dotenv`는 Node.js 환경에서 환경 변수(환경 설정값)를 쉽게 관리하기 위해 사용하는 라이브러리입니다. `.env` 파일에 정의된 환경 변수들을 Node.js 애플리케이션에서 로드하여, 코드 내에서 `process.env`를 통해 접근할 수 있도록 도와줍니다.  

이 라이브러리를 사용하면 환경 변수들을 코드에서 직접 하드코딩하지 않고, 별도의 파일로 분리하여 관리할 수 있어 보안성과 유지보수성이 향상됩니다.

---

### 주요 기능과 장점
1. **환경 변수 관리**  
   `.env` 파일을 통해 애플리케이션의 민감한 데이터(예: API 키, 데이터베이스 URL, 비밀번호 등)를 저장 및 로드합니다.

2. **보안 강화**  
   환경 변수는 코드 저장소에 포함되지 않도록 `.gitignore` 파일을 통해 `.env` 파일을 제외하여 보안을 강화할 수 있습니다.

3. **환경별 설정**  
   개발, 테스트, 운영 환경 등 다양한 환경에 따라 설정값을 관리할 수 있습니다.

4. **사용 간단**  
   몇 줄의 코드만으로 환경 변수 설정을 애플리케이션에 적용할 수 있습니다.

---

### 설치 및 사용법

#### 1. 설치
```bash
npm install dotenv
```

#### 2. `.env` 파일 생성
프로젝트 루트 디렉토리에 `.env` 파일을 생성하고 환경 변수를 정의합니다.  
예:
```env
PORT=3000
DB_HOST=localhost
DB_USER=root
DB_PASS=password123
```

#### 3. 코드에서 `dotenv` 설정
`dotenv`를 초기화하고 환경 변수를 불러옵니다.  
```javascript
require('dotenv').config();

// 환경 변수 사용 예시
const port = process.env.PORT;
const dbHost = process.env.DB_HOST;

console.log(`Server will run on port: ${port}`);
console.log(`Database host: ${dbHost}`);
```

---

### 주요 메서드 및 옵션

#### 1. `config()` 메서드
`dotenv`의 기본 메서드로, `.env` 파일을 읽어서 `process.env`에 환경 변수를 추가합니다.  
옵션으로 다른 파일 경로나 설정을 지정할 수 있습니다.

```javascript
require('dotenv').config({
  path: './config/.env', // 기본값: './.env'
  encoding: 'utf8',      // 파일 인코딩 (기본값: 'utf8')
  debug: true            // 디버깅 정보 출력
});
```

#### 2. 파싱(`parse()`)
파일 내용을 직접 문자열로 전달하여 파싱할 수 있습니다.
```javascript
const dotenv = require('dotenv');
const envConfig = dotenv.parse('PORT=8080\nNODE_ENV=development');
console.log(envConfig); // { PORT: '8080', NODE_ENV: 'development' }
```

---

### `.env` 파일 주의사항
1. **형식**  
   - `KEY=VALUE` 형태로 작성하며, 공백 없이 정의해야 합니다.
   - 값에 특수문자가 포함될 경우, 따옴표로 감쌀 수 있습니다.
     ```env
     PASSWORD="my@secure$pwd"
     ```

2. **주석 사용**  
   `#`로 시작하는 행은 주석으로 처리됩니다.
   ```env
   # 이 줄은 주석입니다
   PORT=3000
   ```

3. **환경 변수는 문자열로 저장됨**  
   `.env` 파일에서 설정된 모든 값은 문자열로 처리됩니다.

---

### `.env` 파일 관리 팁
1. **`.gitignore`에 추가**  
   `.env` 파일을 버전 관리에서 제외해야 민감한 정보가 외부에 노출되지 않습니다.  
   `.gitignore` 파일:
   ```
   .env
   ```

2. **환경별 설정 파일**  
   각 환경에 따라 다른 `.env` 파일을 사용할 수 있습니다.
   - `.env.development`
   - `.env.production`
   - `.env.test`

   코드에서 동적으로 파일을 선택하도록 설정:
   ```javascript
   const envFile = process.env.NODE_ENV === 'production' ? '.env.production' : '.env.development';
   require('dotenv').config({ path: envFile });
   ```

---

### 주의사항
- `dotenv`는 프로덕션 환경에서 사용하는 것이 아니라 **로컬 개발** 및 간단한 환경 변수 관리를 위해 설계되었습니다.
- 클라우드 환경(AWS, Heroku 등)에서는 해당 플랫폼이 제공하는 환경 변수 관리 기능을 사용하는 것이 권장됩니다.

---

`dotenv`는 Node.js 프로젝트에서 환경 변수 관리의 필수 도구 중 하나로 간단하면서도 강력한 기능을 제공합니다. 🎯

