---
title: "NPM 패키지 연습: franc, langs, colors로 언어 예측 프로그램 만들기"
excerpt: "Node.js에서 franc, langs, colors 패키지를 활용하여 간단한 언어 예측 프로그램을 만들어보고 NPM 설치 및 활용 방법을 배워봅니다."
categories:
  - Node
tags:
  - [Node.js, NPM, JavaScript, Backend]
permalink: /Node/npm-language-detector/
toc: true
toc_sticky: true
date: 2024-12-8
last_modified_at: 2024-12-8
---

Node.js를 사용하여 언어 예측 프로그램을 만들어보면서 NPM 패키지 설치 및 활용 방법을 알아보겠습니다. 이 프로젝트에서는 `franc`, `langs`, 그리고 `colors` 패키지를 활용합니다.

---

## 1. 환경 설정

### 1.1 Node.js 설치
Node.js가 설치되어 있지 않다면 [Node.js 공식 웹사이트](https://nodejs.org)에서 다운로드하여 설치합니다.  
설치 후, 터미널에서 다음 명령어로 Node.js와 npm의 버전을 확인합니다.

```bash
node -v
npm -v
```

### 1.2 프로젝트 디렉토리 생성 및 초기화
다음 명령어를 통해 프로젝트 디렉토리를 생성하고 초기화합니다.

```bash
mkdir language-detector
cd language-detector
npm init -y
```

`npm init -y`는 `package.json` 파일을 생성하며 기본 설정값을 사용합니다.

---

## 2. NPM 패키지 설치

이 프로젝트에서는 다음 패키지들을 설치합니다:
- **`franc`**: 텍스트의 언어 코드를 추측.
- **`langs`**: 언어 코드를 기반으로 언어 이름을 조회.
- **`colors`**: 터미널 텍스트에 색상을 추가.

### 패키지 설치 명령어

```bash
npm install franc langs colors
```

---

## 3. 코드 설명

아래 코드는 언어 예측 프로그램의 예제입니다. 이 코드는 명령줄에서 입력받은 텍스트를 분석하여 해당 언어를 예측하고 결과를 출력합니다.

```js
const franc = require("franc");
const langs = require("langs");
const colors = require("colors");

const input = process.argv[2];
const langCode = franc(input);
if (langCode === 'und' || langCode === 'sco') {
    console.log('Please input more letters as 10 or more!'.red);
} else {
    const langName = langs.where("3", langCode);
    console.log(`Our best guess is : ${langName.name}`.green);
}
```

### 코드 동작 방식
1. **텍스트 입력받기**: `process.argv[2]`를 통해 명령줄에서 텍스트를 입력받습니다.
2. **언어 코드 감지**: `franc` 패키지가 텍스트를 분석하여 ISO 639-3 형식의 언어 코드를 반환합니다.
3. **언어 이름 조회**: `langs` 패키지를 이용하여 감지된 언어 코드를 언어 이름으로 변환합니다.
4. **결과 출력**: `colors` 패키지를 활용해 결과 메시지를 색상으로 표시합니다.

---

## 4. 실행 방법

1. 작성한 코드를 `index.js`라는 이름으로 저장합니다.
2. 명령어를 실행합니다:
   ```bash
   node index.js "Sample text here"
   ```
3. 출력 결과:
   - 텍스트가 충분히 길면 감지된 언어가 초록색으로 출력됩니다.
   - 텍스트가 짧거나 감지할 수 없는 경우 빨간색 메시지가 표시됩니다.

---

## 5. 실행 예제

### 예제 1: 텍스트 감지 성공
```bash
node index.js "Bonjour tout le monde"
```
출력:
```plaintext
Our best guess is : French
```

### 예제 2: 텍스트 감지 실패
```bash
node index.js "abc"
```
출력:
```plaintext
Please input more letters as 10 or more!
```

---

## 6. 확장 아이디어

1. **입력 파일 읽기**: 텍스트 파일을 읽어서 언어를 감지하는 기능 추가.
2. **UI 통합**: React와 같은 프런트엔드 라이브러리를 결합하여 웹 기반 인터페이스를 추가.
3. **다국어 지원**: 감지 결과를 다양한 언어로 출력하도록 기능 확장.
4. **언어 필터링**: 특정 언어만 감지하도록 `whitelist` 옵션 활용.

### 언어 필터링 예제
```js
const franc = require("franc");
const whitelist = ["eng", "spa", "fra"]; // 영어, 스페인어, 프랑스어만 감지
const langCode = franc("Bonjour", { whitelist });
console.log(langCode); // "fra"
```

이 코드와 함께 Node.js 및 NPM을 활용하여 더욱 다양한 프로젝트를 실습해볼 수 있습니다. 질문이 있다면 언제든지 댓글로 남겨주세요!
