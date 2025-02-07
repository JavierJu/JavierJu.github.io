---
title: "Codesandbox, VS Code, GitHub 연계 사용법: 효율적인 워크플로우 구축"
excerpt: "Codesandbox, VS Code, GitHub을 연계하여 효율적인 개발 환경을 구축하는 방법을 알아봅니다. GitHub 저장소와 동기화하여 협업하고, VS Code에서 로컬 개발을 진행하는 과정까지 상세히 설명합니다."
categories:
  - 개발 환경
  - Git
  - 클라우드 개발
  - Web Development
  - VS Code
tags:
  - [Codesandbox, VS Code, GitHub, Web Development, 협업]
permalink: /VS Code/codesandbox-vscode-github-workflow/
toc: true
toc_sticky: true
date: 2025-02-07
last_modified_at: 2025-02-07
---

## 1. 전체적인 흐름
Codesandbox(CSB)는 클라우드 기반의 코드 편집기로, 브라우저에서 바로 개발이 가능하지만 VS Code와 GitHub을 함께 사용하면 더 효율적인 개발 워크플로우를 구축할 수 있습니다.

### **✅ 기본적인 사용 흐름**
1. **CSB에서 프로젝트 생성** → GitHub 저장소와 연동
2. **VS Code에서 CSB 프로젝트를 로컬에 클론**
3. **VS Code에서 개발 후 GitHub에 푸시**
4. **CSB에서 최신 변경 사항 동기화 및 배포**

이 과정을 순차적으로 설명하겠습니다.

---

## 2. Codesandbox에서 프로젝트 생성 및 GitHub 연동

### **1️⃣ Codesandbox에서 프로젝트 생성**
1. [Codesandbox](https://codesandbox.io) 접속 후 GitHub 계정으로 로그인합니다.
2. 새로운 Sandbox(프로젝트)를 생성합니다.
3. 좌측 사이드바에서 **Git 아이콘** 클릭 → **"Connect repository"** 선택
4. GitHub 인증 후, 새 리포지토리를 생성하거나 기존 리포지토리에 연결합니다.

### **2️⃣ GitHub에 코드 업로드 (Push)**
CSB에서 GitHub 저장소와 연결되면, 변경 사항을 커밋하고 GitHub에 푸시할 수 있습니다.

```sh
git add .
git commit -m "Initial commit from Codesandbox"
git push origin main
```

이제 CSB에서 만든 프로젝트가 GitHub에 저장되었습니다.

---

## 3. VS Code에서 프로젝트 클론 및 개발
GitHub에 저장된 프로젝트를 로컬 환경에서 VS Code로 가져와서 개발을 진행할 수 있습니다.

### **1️⃣ VS Code에서 GitHub 프로젝트 클론**
1. **GitHub에서 저장소 URL 복사**
   - GitHub에서 프로젝트 페이지로 이동 후 `Code` 버튼 클릭 → `HTTPS` 또는 `SSH` URL 복사
2. **VS Code에서 터미널 실행 후 클론**

```sh
git clone <리포지토리 URL>
cd <프로젝트 폴더>
code .
```

3. **프로젝트 환경 설정**
   - `.env` 파일 설정, `npm install` 실행 등 개발 환경을 세팅합니다.

```sh
npm install
```

이제 VS Code에서 개발을 진행할 준비가 완료되었습니다.

---

## 4. VS Code에서 코드 수정 후 GitHub에 푸시
VS Code에서 프로젝트를 수정한 후, GitHub으로 푸시하면 CSB에서도 동기화할 수 있습니다.

### **1️⃣ 변경 사항 확인**
```sh
git status
```

### **2️⃣ 변경 사항 스테이징**
```sh
git add .
```

### **3️⃣ 커밋**
```sh
git commit -m "update: 코드 수정"
```

### **4️⃣ GitHub에 푸시**
```sh
git push origin main
```

이제 GitHub에 코드가 업로드되었으며, CSB에서도 최신 코드로 업데이트할 수 있습니다.

---

## 5. CSB에서 변경 사항 동기화 및 배포
GitHub에 푸시한 코드 변경 사항은 CSB에서도 자동으로 반영됩니다.

### **✅ 최신 코드 동기화하는 방법**
1. CSB에서 **Git 아이콘 클릭**
2. **"Pull latest changes"** 버튼 클릭하여 최신 코드 가져오기

### **✅ CSB에서 프로젝트 실행 및 배포**
1. CSB에서 프로젝트를 실행하여 동작을 확인합니다.
2. 배포 기능을 활용하여 프론트엔드 프로젝트를 손쉽게 배포할 수 있습니다.

---

## 6. 추천 워크플로우

### 🚀 **CSB + VS Code + GitHub 활용법 요약**
- CSB에서 **빠르게 프로토타이핑** → GitHub 저장소에 커밋
- VS Code에서 **본격적인 개발 진행** → GitHub에 푸시
- 필요하면 CSB에서 **최신 코드 동기화 및 실행**

이렇게 하면 클라우드와 로컬 개발 환경을 연계하여 효율적으로 개발할 수 있습니다. 😊

