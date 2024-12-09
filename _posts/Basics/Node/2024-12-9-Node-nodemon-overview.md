---
title: "nodemon: Node.js 개발 효율을 높이는 도구"
excerpt: "Node.js 애플리케이션 개발 중 코드 변경을 감지하고 자동으로 다시 시작해 주는 nodemon에 대해 알아봅니다."
categories:
  - Node
tags:
  - [Node.js, Backend, 개발 도구]
permalink: /Node/nodemon-overview/
toc: true
toc_sticky: true
date: 2024-12-9
last_modified_at: 2024-12-9
---

`nodemon`은 Node.js 애플리케이션 개발 중 코드 변경을 감지하여 애플리케이션을 자동으로 다시 시작해 주는 도구입니다. 개발 환경에서 특히 유용하며, 수동으로 서버를 다시 시작할 필요 없이 코드 변경 내용을 즉시 확인할 수 있습니다.

## 주요 특징
1. **자동 재시작**: 파일이 변경될 때 Node.js 애플리케이션을 자동으로 다시 시작합니다.
2. **파일 확장자 감시**: 기본적으로 `.js`, `.mjs`, `.json` 파일 변경을 감지하며, 다른 파일 확장자도 감시 설정이 가능합니다.
3. **설정 파일 지원**: `nodemon.json` 파일을 사용하여 구성 설정을 저장할 수 있습니다.
4. **명령어 지원**: Node.js 실행 외에도 다른 스크립트나 명령어 실행이 가능합니다.

---

## 설치 방법
전역 또는 로컬로 설치할 수 있습니다.

1. **전역 설치**:
   ```bash
   npm install -g nodemon
   ```

   전역 설치 후 어디서나 `nodemon` 명령어를 사용할 수 있습니다.

2. **로컬 설치**:
   ```bash
   npm install --save-dev nodemon
   ```

   로컬 설치는 프로젝트의 개발 의존성(`devDependencies`)에 추가됩니다.

---

## 사용법
1. **애플리케이션 시작**:
   ```bash
   nodemon app.js
   ```

2. **특정 파일 확장자 감시**:
   ```bash
   nodemon --ext js,html
   ```

3. **특정 디렉터리 감시 제외**:
   ```bash
   nodemon --ignore node_modules
   ```

4. **스크립트로 실행** (package.json에 추가):
   ```json
   "scripts": {
     "start": "nodemon app.js"
   }
   ```

   실행:
   ```bash
   npm run start
   ```

---

## 설정 파일
`nodemon.json` 파일을 프로젝트 루트에 생성하여 설정을 관리할 수 있습니다.

예제:
```json
{
  "watch": ["src", "config"],
  "ext": "js,json",
  "ignore": ["node_modules"],
  "exec": "node app.js"
}
```

---

## 유용한 옵션
- `--watch`: 특정 디렉터리나 파일을 감시.
- `--exec`: 기본 `node` 외의 명령 실행.
- `--delay`: 파일 변경 후 지연 시간을 설정 (예: `--delay 2`).
- `--quiet`: 출력 로그를 최소화.

---

## 참고
개발 환경에서 매우 유용하지만, 프로덕션 환경에서는 사용하지 않는 것이 좋습니다. 프로덕션 환경에서는 PM2 같은 프로세스 관리 도구를 고려하세요.
