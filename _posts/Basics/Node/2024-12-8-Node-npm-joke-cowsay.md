---
title: "NPM 패키지 연습: give-me-a-joke, cowsay, colors로 랜덤 농담 출력하기"
excerpt: "Node.js에서 NPM 패키지 give-me-a-joke, cowsay, colors를 사용하여 랜덤 농담을 ASCII 아트로 출력하는 연습 과정을 단계별로 알아봅니다."
categories:
  - Node
tags:
  - [JavaScript, Node.js, NPM, 패키지 활용]
permalink: /Node/npm-joke-cowsay/
toc: true
toc_sticky: true
date: 2024-12-8
last_modified_at: 2024-12-8
---

Node.js와 NPM 패키지를 활용하여 간단한 프로젝트를 완성해 봅시다. 이번 예제에서는 `give-me-a-joke`로 랜덤 농담을 생성하고, `cowsay`로 출력하며, `colors`를 이용해 텍스트에 색상을 입히는 과정을 단계별로 소개합니다.

## 1. 프로젝트 초기화

### 1.1. 디렉토리 생성 및 초기화
먼저, 새 디렉토리를 만들고 Node.js 프로젝트를 초기화합니다:
```
bash
mkdir joke-cowsay-demo
cd joke-cowsay-demo
npm init -y
```

이 명령은 프로젝트를 초기화하고 `package.json` 파일을 생성합니다.

---

## 2. NPM 패키지 설치

### 2.1. 필요한 패키지 설치
다음 명령으로 프로젝트에 필요한 NPM 패키지를 설치합니다:
```
bash
npm install give-me-a-joke cowsay colors
```

설치 후, `node_modules` 디렉토리와 `package-lock.json` 파일이 생성됩니다. `package.json`의 `dependencies` 섹션에는 설치된 패키지가 추가됩니다.

---

## 3. 코드 작성

### 3.1. `index.js` 파일 생성
디렉토리에 `index.js` 파일을 생성하고 다음과 같이 작성합니다:
```
js
// 패키지 로드
const jokes = require("give-me-a-joke");
const colors = require("colors");
const cowsay = require("cowsay");

// 랜덤 농담 생성 후 cowsay로 출력
jokes.getRandomDadJoke(function (joke) {
    console.log(cowsay.say({
        text: joke.rainbow, // 무지개 색 농담 출력
        e: "^^",           // 소의 눈 모양
        T: "U "            // 소의 혀 모양
    }));
});

// cowsay로 테스트 메시지 출력
console.log(cowsay.say({
    text: "Hello, this is a test message!".green, // 초록색 텍스트
    e: "oO",                                    // 소의 눈 모양
    T: "wW"                                     // 소의 혀 모양
}));
```

---

## 4. 코드 실행

### 4.1. 실행
작성한 코드를 다음 명령으로 실행합니다:
```
bash
node index.js
```

### 4.2. 실행 결과
실행하면 아래와 같은 결과를 확인할 수 있습니다 (농담은 매번 랜덤으로 바뀝니다):
```
plaintext
 _______________________
< Why don't skeletons fight each other? They don't have the guts. >
 -----------------------
        ^^
        U
 _______________________________
< Hello, this is a test message! >
 -------------------------------
        oO
        wW
```

---

## 5. 디버깅 및 추가 연습

### 5.1. 디버깅
코드 실행 시 오류가 발생하면 `console.log`를 활용해 각 단계의 변수를 확인하고 수정하세요.

### 5.2. 개선 아이디어
- **다양한 농담 출력**: `give-me-a-joke`의 다른 메서드(`getCustomJoke`, `getChuckNorrisJoke`)를 사용해보세요.
- **스타일링**: `colors`를 활용해 농담에 다양한 색상을 입혀보세요.
- **함수화**: 각 기능을 함수로 분리하여 코드를 구조화해보세요.

---

이 과정을 통해 Node.js와 NPM 패키지를 활용한 간단한 프로젝트를 성공적으로 완성할 수 있습니다. 앞으로도 다양한 패키지를 연습하며 활용 범위를 넓혀보세요! 😊
