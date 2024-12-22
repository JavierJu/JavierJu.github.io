---
title: "Morgan-Logger: Express 애플리케이션에서 HTTP 요청 로깅하기"
excerpt: "Morgan 미들웨어의 개요, 설치 방법, 예제 코드, 장단점을 알아보고 Node.js 애플리케이션에서 효과적으로 사용하는 방법을 소개합니다."
categories:
  - Node
tags:
  - [JavaScript, Node.js, Backend, Express, HTTP, Logging]
permalink: /Node/morgan-logger-overview/
toc: true
toc_sticky: true
date: 2024-12-22
last_modified_at: 2024-12-22
---

### **개요**
**모건(Morgan)**은 Node.js와 Express 애플리케이션에서 HTTP 요청을 로깅하는 미들웨어입니다. 서버에 들어오는 요청과 관련된 정보를 간단하고 체계적으로 기록하여 디버깅, 모니터링, 분석에 활용할 수 있습니다.

#### **특징**
- **역할:** HTTP 요청 정보를 로깅.
- **장점:**
  - 다양한 사전 정의된 로그 형식 제공 (`tiny`, `dev`, `common` 등).
  - 커스터마이징 가능.
  - 요청, 응답, 상태 코드, 응답 시간 등의 정보를 기록.
- **사용 이유:**
  - 애플리케이션 동작 및 오류를 추적.
  - 서버의 상태를 모니터링.
  - 디버깅 및 퍼포먼스 개선.

---

### **설치 방법**
Morgan은 NPM을 통해 설치합니다:

```bash
npm install morgan
```

---

### **기본 사용법**
다음은 Express 애플리케이션에서 Morgan을 사용하는 기본 코드입니다:

```javascript
const express = require('express');
const morgan = require('morgan');

const app = express();

// 요청 로깅 미들웨어 등록
app.use(morgan('dev')); // 'dev'는 개발용 간단한 로그 형식

app.get('/', (req, res) => {
  res.send('Hello World!');
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

---

### **로그 형식**
Morgan은 여러 가지 사전 정의된 로그 형식을 제공합니다:

1. **dev**: 개발 환경에 적합한 짧고 컬러풀한 로그.
2. **common**: 일반적인 웹 로그 형식 (클라이언트 IP, 메서드, 경로 등).
3. **combined**: Apache 로그 형식 (보다 상세한 정보 포함).
4. **tiny**: 간단한 요약 로그.
5. **short**: 기본적인 요청 요약.

```javascript
app.use(morgan('combined')); // Apache 스타일 로그 형식
```

---

### **커스터마이징**
#### **1. 로그 저장하기**
로깅 데이터를 파일에 저장하려면 `fs` 모듈과 함께 사용할 수 있습니다:

```javascript
const fs = require('fs');
const path = require('path');
const morgan = require('morgan');

// 로그 파일 스트림 생성
const accessLogStream = fs.createWriteStream(path.join(__dirname, 'access.log'), {
  flags: 'a', // 추가 모드
});

// 로그를 파일에 저장
app.use(morgan('combined', { stream: accessLogStream }));
```

#### **2. 사용자 정의 포맷**
요청과 응답 정보를 커스터마이즈해서 로깅할 수 있습니다:

```javascript
morgan.token('id', (req) => req.id); // 커스텀 토큰
app.use(
  morgan(':method :url :status :res[content-length] - :response-time ms :id')
);
```

---

### **언제 사용하는가?**
- 개발 환경에서 서버의 요청 및 응답을 실시간으로 모니터링.
- 운영 환경에서 요청 로그를 저장하여 분석 및 디버깅.
- 성능 문제를 추적하기 위한 응답 시간 측정.
- 이상 행동(예: 404 또는 500 에러)의 패턴 분석.

---

### **장점**
1. **간편성**: 설정 및 사용이 매우 간단.
2. **가독성**: 사전 정의된 포맷으로 읽기 쉬운 로그 제공.
3. **확장성**: 사용자 정의 포맷 및 토큰으로 맞춤형 로깅 가능.
4. **통합성**: Express 및 기타 Node.js 프레임워크와 쉽게 통합.
5. **성능 최적화**: 비동기 방식으로 동작하여 서버 성능에 큰 영향을 미치지 않음.

---

### **단점**
1. **심화 로깅 부족**: 로그의 세부 사항이 제한적이며, 복잡한 로깅 요구에는 부족할 수 있음.
   - 해결 방법: Winston이나 Bunyan 같은 고급 로깅 라이브러리와 함께 사용.
2. **운영 환경 한계**: 기본적으로 파일 저장보다는 콘솔 로깅에 적합.
3. **추가 디버깅 기능 부족**: APM(Application Performance Monitoring) 수준의 기능은 제공하지 않음.

---

### **Winston과의 통합 예제**
Morgan과 Winston을 조합하여 더 강력한 로깅 시스템을 구축할 수도 있습니다:

```javascript
const express = require('express');
const morgan = require('morgan');
const winston = require('winston');

const app = express();

// Winston 설정
const logger = winston.createLogger({
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.Console(),
  ],
});

// Morgan을 사용하여 Winston과 통합
app.use(
  morgan('combined', {
    stream: {
      write: (message) => logger.info(message.trim()), // Morgan 로그를 Winston으로 전달
    },
  })
);

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

---

### **결론**
Morgan은 간단한 로깅 요구에 적합한 미들웨어로, 개발 및 운영 환경에서 HTTP 요청을 모니터링하고 분석하는 데 유용합니다. 더 고급 로깅이 필요하다면 Morgan과 다른 로깅 라이브러리를 조합하여 사용하는 것이 권장됩니다.

