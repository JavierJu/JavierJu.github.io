---
title: "VS Code에서 JavaScript 코드를 실행하고 테스트하는 방법"
excerpt: "VS Code에서 JavaScript 코드를 실행하여 결과를 확인하는 다양한 방법에 대해 알아봅니다."
categories:
  - VS Code
tags:
  - [JavaScript, VS Code, Node.js, Development Tools]
permalink: /vs-code/vscode-javascript-testing/
toc: true
toc_sticky: true
date: 2024-11-18
last_modified_at: 2024-11-18
---

JavaScript 코드를 테스트할 때 보통 Chrome DevTools를 사용하는 경우가 많지만, VS Code에서도 효율적으로 테스트할 수 있는 다양한 방법이 있습니다. 이 글에서는 **Code Runner**, **Node.js**, **Quokka.js**, 그리고 **Live Server**를 활용하여 JavaScript 코드를 실행하는 방법을 알아보겠습니다.

---

## 1. Code Runner 확장 프로그램 활용하기

VS Code에서 Code Runner를 사용하면 쉽게 JavaScript 코드를 실행할 수 있습니다. 하지만 기본 설정에서는 출력이 **OUTPUT 탭**에 나타나기 때문에 결과 확인이 어려울 수 있습니다.

### 실행 방법
1. **`console.log`로 출력값을 추가**합니다:
   """
   const obj = { name: "John", age: 30 };
   console.log(obj);
   """
2. **Run Code** 버튼을 클릭하거나 `Ctrl+Alt+N`을 눌러 실행합니다.
3. 결과를 **TERMINAL 탭**에서 확인하려면 다음 설정을 변경하세요:
   - **File > Preferences > Settings**를 엽니다.
   - 검색창에 `Code Runner: Run In Terminal`을 입력합니다.
   - 해당 옵션을 **체크**합니다.

---

## 2. Node.js 터미널 직접 사용하기

Node.js가 설치되어 있다면 JavaScript 파일을 Node.js 환경에서 바로 실행할 수 있습니다.

### 실행 방법
1. VS Code의 **TERMINAL 탭**에서 현재 디렉토리를 설정합니다:
   """
   cd <작업 디렉토리>
   """
2. 파일 실행 명령어를 입력합니다:
   """
   node 파일명.js
   """
3. 터미널에 출력된 결과를 확인합니다.

---

## 3. Quokka.js로 실시간 테스트

**Quokka.js**는 JavaScript 코드를 실시간으로 실행하고 결과를 확인할 수 있는 VS Code 확장 프로그램입니다.

### 설치 및 사용 방법
1. VS Code **Extensions**에서 `Quokka.js`를 검색하여 설치합니다.
2. 새 JavaScript 파일을 열고 Command Palette (`Ctrl+Shift+P`)에서 **Quokka: Start on Current File**을 선택합니다.
3. 작성한 코드의 결과가 에디터 오른쪽에 실시간으로 표시됩니다:
   """
   const x = 5;
   const y = 10;
   console.log(x + y); // 결과: 15
   """

---

## 4. Live Server와 Chrome DevTools 사용하기

HTML과 연동된 JavaScript 코드를 테스트하는 경우, **Live Server**를 활용하면 브라우저에서 코드를 실시간으로 실행하고 Chrome DevTools를 통해 결과를 확인할 수 있습니다.

### 실행 방법
1. **Live Server** 확장 프로그램을 설치합니다.
2. HTML 파일을 열고 **Go Live** 버튼을 클릭합니다.
3. 브라우저에서 페이지를 열고 Chrome DevTools (`F12`)를 사용해 JavaScript 결과를 확인합니다.

---

## 결론

VS Code에서 JavaScript 코드를 테스트하는 방법은 다양합니다. 간단한 코드 실행에는 **Code Runner**나 **Node.js 터미널**을, 실시간 결과 확인에는 **Quokka.js**, 그리고 브라우저와의 연동에는 **Live Server**를 활용하세요. 자신의 필요에 맞는 방법을 선택하면 JavaScript 코딩 경험이 더욱 편리해질 것입니다.
