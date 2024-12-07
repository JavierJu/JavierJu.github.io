---
title: "Node.js 버전 관리: 확인, 업데이트, 주요 차이점, 그리고 NVM 사용법"
excerpt: "Node.js 버전을 확인하고, 업데이트하며, 주요 차이점을 이해하는 방법을 살펴봅니다. 또한, NVM(Node Version Manager)을 사용해 다양한 Node.js 버전을 손쉽게 관리하는 방법도 소개합니다."
categories:
  - Node
tags:
  - [JavaScript, Node.js, Backend, NVM, 개발 도구]
permalink: /Node/nodejs-version-management/
toc: true
toc_sticky: true
date: 2024-12-07
last_modified_at: 2024-12-07
---

## Node.js 버전 확인 및 업데이트

### Node.js 버전 확인
Node.js는 빠른 업데이트와 릴리즈 주기로 유명합니다. 현재 설치된 Node.js의 버전을 확인하려면 아래 명령어를 터미널에서 실행하세요:

```bash
node -v
```

이 명령은 설치된 Node.js 버전을 출력합니다. 예를 들어, `v18.17.1`과 같은 형식으로 표시됩니다.

### Node.js 업데이트
Node.js를 최신 버전으로 업데이트하려면 다음 방법 중 하나를 사용할 수 있습니다.

1. **패키지 관리자를 사용하는 방법 (Linux/Mac)**
   ```bash
   sudo npm install -g n
   sudo n stable
   ```
   위 명령은 최신 안정 버전(stable)을 설치합니다.

2. **Windows에서 업데이트**
   Windows 사용자는 [Node.js 공식 사이트](https://nodejs.org/)에서 최신 설치 파일을 다운로드하여 설치하면 됩니다.

3. **NVM(Node Version Manager) 사용**
   다양한 Node.js 버전을 손쉽게 관리하려면 NVM을 사용하는 것이 가장 편리합니다. 자세한 내용은 아래 NVM 섹션에서 다룹니다.

---

## Node.js 버전별 주요 차이점
Node.js는 Long Term Support(LTS)와 Current 두 가지 릴리즈 트랙으로 제공됩니다.

- **LTS(Long Term Support)**: 안정성과 장기 지원을 제공하며, 프로덕션 환경에 적합합니다.
- **Current**: 최신 기능과 개선 사항이 포함되어 있으며, 개발 중 실험적인 테스트에 적합합니다.

### 주요 릴리즈별 변화
1. **Node.js 16 (LTS)**
   - V8 JavaScript 엔진 9.x 도입.
   - Apple Silicon 지원.
   - `Promise.any()`와 `String.replaceAll()` 메서드 추가.

2. **Node.js 18 (LTS)**
   - Fetch API 정식 지원.
   - Test Runner(`node:test` 모듈) 추가.
   - OpenSSL 3.0 지원.

3. **Node.js 20**
   - Permission Model 도입(실험적).
   - V8 11.x로 업데이트.
   - 파일 시스템 속도 개선.

---

## NVM(Node Version Manager) 사용법

NVM은 Node.js의 다양한 버전을 쉽게 설치하고 전환할 수 있는 도구입니다.

### NVM 설치
NVM은 GitHub에서 제공되며, 설치는 간단합니다.

1. **Linux/Mac**
   ```bash
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
   ```
   설치가 완료되면 `~/.bashrc` 또는 `~/.zshrc`에 다음 줄을 추가하세요:
   ```bash
   export NVM_DIR="$HOME/.nvm"
   [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

2. **Windows**
Windows 사용자는 [nvm-windows](https://github.com/coreybutler/nvm-windows)를 사용할 수 있습니다. 설치 후 명령 프롬프트 또는 PowerShell에서 아래 명령어를 사용하여 Node.js 버전을 관리할 수 있습니다.

---

### NVM 주요 명령어 (Windows)
- **Node.js 설치**  
  ```bash
  nvm install <버전 번호>
  ```
  예: `nvm install 18.17.1`

- **설치된 Node.js 버전 확인**  
  ```bash
  nvm list
  ```

- **사용할 버전 전환**  
  ```bash
  nvm use <버전 번호>
  ```

- **특정 버전 제거**  
  ```bash
  nvm uninstall <버전 번호>
  ```

---

## 마무리
Node.js 버전 관리는 안정적인 프로젝트 진행과 최신 기능 활용을 위해 필수적입니다. 특히 NVM을 사용하면 다양한 프로젝트에서 요구하는 Node.js 버전을 손쉽게 전환할 수 있어 개발 환경을 더욱 유연하게 설정할 수 있습니다. 이번 글이 효과적인 Node.js 관리에 도움이 되길 바랍니다!
