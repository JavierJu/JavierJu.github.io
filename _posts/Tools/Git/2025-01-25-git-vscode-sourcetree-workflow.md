---
title: "VS Code와 SourceTree를 함께 사용하는 효율적인 Git 워크플로우"
excerpt: "VS Code와 SourceTree를 함께 사용하여 개발과 Git 소스 관리를 효율적으로 처리하는 방법을 소개합니다. 두 도구의 장점과 연동 방법을 코드 예제와 함께 설명합니다."
categories:
  - Git
  - Tools
  - Workflow
  - VS Code
  - SourceTree
tags:
  - [Git, VSCode, SourceTree, Workflow, 개발 도구]
permalink: /git/vscode-sourcetree-workflow/
toc: true
toc_sticky: true
date: 2025-01-25
last_modified_at: 2025-01-25
---

## VS Code와 SourceTree를 함께 사용하는 방법

**VS Code**는 강력한 코드 에디터로서 Git과 기본 통합 기능을 제공하며, **SourceTree**는 브랜치 관리와 시각적 Git 히스토리 확인에 유용한 도구입니다. 두 도구를 함께 사용하면 개발 생산성을 극대화할 수 있습니다.

---

### VS Code의 Git 기능 요약

**장점:**
- 코드 작성과 Git 관리가 한 화면에서 가능.
- GitLens와 같은 확장 프로그램으로 추가적인 기능 제공.

**단점:**
- 복잡한 브랜치 히스토리를 시각적으로 확인하기 어려움.
- 대규모 협업 프로젝트에서는 GUI의 한계 존재.

---

### SourceTree의 주요 기능

**장점:**
- 시각적으로 브랜치 트리와 히스토리를 확인 가능.
- 초보자에게 친숙한 GUI 제공.
- 복잡한 충돌 해결과 브랜치 병합에 강점.

---

### 두 도구의 조합
VS Code를 코드 에디터로 사용하고, SourceTree를 Git 관리 도구로 병행하여 사용하는 것은 다음과 같은 장점이 있습니다:

1. **작업 분리**: 코드 작업은 VS Code, 브랜치 및 커밋 관리는 SourceTree에서.
2. **효율성 향상**: 프로젝트 파일 변경사항이 실시간으로 두 도구에 반영.
3. **시각화**: 복잡한 브랜치 히스토리도 SourceTree에서 쉽게 확인 가능.

---

## 워크플로우 설정 및 연동 방법

### 1. 프로젝트 디렉토리 설정

1. **VS Code**에서 프로젝트 디렉토리를 엽니다.
   ```bash
   code /path/to/project
   ```

2. 동일한 디렉토리를 **SourceTree**에서 열어 Git 관리를 시작합니다.

### 2. SourceTree에서 VS Code 연동 설정

SourceTree에서 VS Code를 기본 편집기로 설정하면 Git 작업 중 파일을 빠르게 VS Code에서 열 수 있습니다.

1. SourceTree 메뉴에서 **도구 > 옵션 > Git > 기본 편집기**로 이동합니다.
2. **VS Code 실행 파일**을 선택합니다:
   - Windows: `C:\Users\YourUser\AppData\Local\Programs\Microsoft VS Code\Code.exe`
   - Mac: `/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code`
3. 설정을 저장합니다.

### 3. 두 도구의 병행 사용

- **VS Code에서 파일 수정**: 수정 후 저장하면 SourceTree에서 변경사항이 표시됩니다.
- **SourceTree에서 브랜치 변경**: 브랜치를 전환하면 VS Code에서도 변경된 상태가 반영됩니다.

---

## 간단한 Git 명령 예제

작업 중 필요에 따라 VS Code의 내장 Git 터미널에서 명령어를 사용할 수도 있습니다:

```bash
# 새로운 브랜치 생성 및 전환
git checkout -b feature/new-feature

# 변경사항 스테이징
git add .

# 커밋
git commit -m "새로운 기능 추가"

# 원격 저장소로 푸시
git push origin feature/new-feature
```

---

## 결론

VS Code와 SourceTree를 함께 사용하는 것은 각 도구의 강점을 극대화하는 훌륭한 방법입니다. **VS Code**는 코드 작성과 간단한 Git 명령에 적합하며, **SourceTree**는 복잡한 브랜치 관리와 시각적 히스토리 확인에 강점이 있습니다.

이 두 도구를 조합하면 효율적인 Git 워크플로우를 구축할 수 있습니다. 지금 바로 설정하고 경험해 보세요! 🚀
