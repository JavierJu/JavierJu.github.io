---
title: "NPM(Node Package Manager): Node.js 패키지 관리의 핵심 도구"
excerpt: "Node.js에서 사용되는 NPM의 주요 기능, 명령어, 파일 구조 등을 자세히 알아봅니다. NPM은 패키지 설치와 관리, 그리고 배포까지 다양한 기능을 제공합니다."
categories:
  - Node
tags:
  - [JavaScript, Node.js, NPM, 패키지 관리]
permalink: /Node/npm-guide/
toc: true
toc_sticky: true
date: 2024-12-7
last_modified_at: 2024-12-7
---

Node.js의 **NPM(Node Package Manager)**은 Node.js 애플리케이션에서 사용할 수 있는 패키지(라이브러리나 모듈)를 관리하는 도구입니다. NPM은 Node.js를 설치하면 기본적으로 함께 설치됩니다. NPM은 개발자들이 코드를 재사용하고 공유하기 위해 만들어졌으며, 전 세계적으로 가장 큰 오픈 소스 라이브러리 저장소입니다.

## **NPM의 주요 기능**
1. **패키지 설치**  
   원하는 Node.js 모듈(패키지)을 설치할 수 있습니다.  
   """
   bash
   npm install [패키지 이름]
   """
   - 기본적으로 설치한 패키지는 `node_modules` 폴더에 저장됩니다.
   - `npm i`로 줄여서 사용할 수 있습니다.

2. **의존성 관리**  
   애플리케이션에서 사용하는 패키지 목록과 버전을 `package.json` 파일에 관리합니다.  
   """
   bash
   npm init
   """
   명령어로 프로젝트를 초기화하면 `package.json`이 생성되며, 이후 설치한 패키지가 여기에 기록됩니다.

3. **스크립트 실행**  
   `package.json` 파일에 정의된 스크립트를 실행할 수 있습니다.  
   예:  
   """
   json
   "scripts": {
     "start": "node app.js",
     "test": "echo 'Running tests...'"
   }
   """
   실행:  
   """
   bash
   npm run start
   npm run test
   """

4. **글로벌 패키지 관리**  
   개발 도구나 CLI(Command Line Interface) 툴은 전역(global)으로 설치하여 어디서든 실행 가능하게 설정할 수 있습니다.  
   """
   bash
   npm install -g [패키지 이름]
   """

5. **패키지 배포**  
   개발자가 만든 패키지를 NPM에 배포하여 다른 사람들이 사용할 수 있게 만들 수 있습니다.  
   """
   bash
   npm publish
   """

## **주요 파일 및 폴더**
1. **`package.json`**  
   프로젝트 메타데이터와 패키지 의존성을 정의한 파일로, 프로젝트의 핵심입니다.  
   주요 필드:
   - `name`: 프로젝트 이름
   - `version`: 버전 정보
   - `dependencies`: 프로덕션용 패키지
   - `devDependencies`: 개발 환경에서만 필요한 패키지

2. **`node_modules`**  
   NPM이 설치한 패키지가 저장되는 디렉토리입니다.

3. **`package-lock.json`**  
   설치한 패키지와 그 의존성의 정확한 버전을 기록한 파일로, 동일한 환경을 재현하는 데 유용합니다.

## **NPM의 주요 명령어**
| 명령어                     | 설명                                                                            |
|----------------------------|---------------------------------------------------------------------------------|
| `npm init`                 | 새로운 `package.json` 파일 생성                                                |
| `npm install` 또는 `npm i` | `package.json`에 정의된 모든 의존성 패키지 설치                                 |
| `npm install [패키지]`     | 특정 패키지 설치 (로컬 설치가 기본)                                             |
| `npm install [패키지] -g`  | 특정 패키지를 전역 설치                                                        |
| `npm uninstall [패키지]`   | 설치된 패키지 제거                                                             |
| `npm update`               | 패키지 업데이트                                                               |
| `npm outdated`             | 사용 중인 패키지의 업데이트 가능한 버전 확인                                   |
| `npm run [스크립트 이름]`  | `package.json`에 정의된 스크립트 실행                                          |
| `npm publish`              | 패키지를 NPM 레지스트리에 배포                                                |
| `npm audit`                | 설치된 패키지의 보안 취약점 스캔                                              |
| `npm cache clean --force`  | 캐시를 정리 (문제 해결 시 유용)                                                |

## **NPM vs Yarn**
NPM은 Yarn과 같은 다른 패키지 매니저와 비교되곤 합니다. Yarn은 페이스북에서 만든 NPM 대안으로, 속도와 안정성을 강조한 도구입니다. 최근 NPM은 버전 7 이상으로 업데이트되며 Yarn과의 주요 기능 차이를 많이 좁혔습니다.

## **추가 학습 자료**
- [공식 NPM 문서](https://docs.npmjs.com/)  
- NPM 사용법을 직접 익혀볼 수 있는 프로젝트: 간단한 Node.js 애플리케이션 개발.

궁금한 점이 있으면 언제든 물어보세요! 😊
