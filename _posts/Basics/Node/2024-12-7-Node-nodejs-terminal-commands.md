---
title: "Git Bash 터미널에서 Node.js 웹 서버 개발 시 유용한 명령어 정리"
excerpt: "Node.js로 웹 서버를 개발할 때 자주 사용하는 Git Bash 터미널 명령어를 Node.js, npm, Git Bash 기본 명령어로 나누어 정리하였습니다."
categories:
  - Node
tags:
  - [JavaScript, Node.js, Terminal, Git Bash, Backend]
permalink: /Node/nodejs-terminal-commands/
toc: true
toc_sticky: true
date: 2024-12-07
last_modified_at: 2024-12-07
---

Node.js로 웹 서버를 개발하거나 관리할 때 Git Bash 터미널에서 자주 사용하는 명령어를 정리했습니다. 이 명령어들은 Node.js 관련 작업, npm 관리, Git Bash 기본 작업으로 나누어 설명합니다. 

---

### **1. Node.js 관련 작업**
#### **Node.js 실행 및 관리**
- `node <파일명>`  
  Node.js 스크립트를 실행합니다.  
  예: `node server.js`

- `node -v` 또는 `node --version`  
  Node.js의 현재 버전을 확인합니다.

- `npm -v` 또는 `npm --version`  
  npm(Node Package Manager)의 버전을 확인합니다.

#### **REPL 사용**
- `node`  
  Node.js REPL(Read-Eval-Print Loop) 환경을 시작합니다.  
  나가려면 `Ctrl + C`를 두 번 누릅니다.

---

### **2. npm 관리 명령어**
#### **패키지 초기화 및 설치**
- `npm init`  
  프로젝트를 초기화하고 `package.json`을 생성합니다.  
  - `npm init -y`: 기본 설정으로 빠르게 초기화합니다.

- `npm install <패키지명>` 또는 `npm i <패키지명>`  
  특정 패키지를 설치합니다.  
  예: `npm install express`

- `npm install`  
  `package.json`에 정의된 모든 의존성을 설치합니다.

#### **개발 환경용 패키지 설치**
- `npm install <패키지명> --save-dev` 또는 `npm i <패키지명> -D`  
  개발 의존성으로 패키지를 설치합니다.  
  예: `npm install nodemon --save-dev`

#### **글로벌 패키지 설치**
- `npm install -g <패키지명>`  
  패키지를 전역으로 설치합니다.  
  예: `npm install -g nodemon`

#### **패키지 업데이트 및 삭제**
- `npm update <패키지명>`  
  특정 패키지를 업데이트합니다.

- `npm uninstall <패키지명>` 또는 `npm remove <패키지명>`  
  설치된 패키지를 삭제합니다.

#### **스크립트 실행**
- `npm run <스크립트명>`  
  `package.json`에 정의된 스크립트를 실행합니다.  
  예: `npm run start`

---

### **3. Git Bash 기본 명령어**
#### **파일 및 폴더 관리**
- `ls`  
  현재 디렉토리의 파일 및 폴더 목록을 표시합니다.

- `cd <디렉토리명>`  
  특정 디렉토리로 이동합니다.  
  예: `cd my-project`

- `mkdir <폴더명>`  
  새로운 디렉토리를 생성합니다.  
  예: `mkdir server`

- `rm <파일명>`  
  파일을 삭제합니다.  
  디렉토리를 삭제하려면 `rm -r <폴더명>`을 사용합니다.

#### **파일 내용 확인**
- `cat <파일명>`  
  파일 내용을 출력합니다.  
  예: `cat server.js`

#### **포트 확인 및 프로세스 종료**
- `netstat -an | grep <포트번호>`  
  특정 포트가 사용 중인지 확인합니다.  
  예: `netstat -an | grep 3000`

- `kill <프로세스ID>`  
  특정 프로세스를 종료합니다.  
  예: `kill 1234`

#### **Git 사용 명령어**
- `git init`  
  Git 저장소를 초기화합니다.

- `git add .`  
  모든 변경 사항을 스테이징합니다.

- `git commit -m "<커밋 메시지>"`  
  변경 사항을 커밋합니다.

- `git push`  
  원격 저장소에 푸시합니다.

---

### **4. 개발 및 디버깅 관련 도구**
#### **nodemon 사용 (자동 재시작)**
- `nodemon <파일명>`  
  코드 변경 시 서버를 자동으로 재시작합니다.  
  예: `nodemon server.js`

#### **환경 변수 관리**
- `export <변수명>=<값>`  
  환경 변수를 설정합니다.  
  예: `export NODE_ENV=development`

- `echo $<변수명>`  
  환경 변수의 값을 출력합니다.  
  예: `echo $NODE_ENV`

---

이 명령어들을 활용하여 Node.js 프로젝트를 효율적으로 관리하고, Git Bash 터미널에서 개발 워크플로우를 간소화할 수 있습니다.
