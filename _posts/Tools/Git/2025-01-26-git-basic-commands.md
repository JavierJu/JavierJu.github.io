---
title: "Git 기본 명령어 완벽 가이드"
excerpt: "Git의 기본 명령어를 자세히 알아보고, 각 명령어의 사용 예제와 함께 Git 워크플로우를 정리합니다. Git을 처음 접하거나 복습하려는 개발자를 위한 완벽 가이드."
categories:
  - Git
  - Version Control
  - Development Tools
tags:
  - [Git, Version Control, 개발 도구, 명령어 가이드, Git 워크플로우]
permalink: /git/basic-commands/
toc: true
toc_sticky: true
date: 2025-01-26
last_modified_at: 2025-02-14
---

## Git 기본 명령어 완벽 가이드

Git은 개발자들이 코드 버전 관리를 위해 사용하는 가장 강력한 도구 중 하나입니다. 이 문서에서는 Git의 기본 명령어를 상황별로 정리하고, 코드 예제를 포함하여 상세히 설명합니다.

---

## 1. Git 초기 설정

### **`git config`**

Git을 사용하기 전, 사용자 정보를 설정해야 합니다. 이 설정은 커밋 기록에 반영됩니다.

```bash
git config --global user.name "사용자 이름"
git config --global user.email "이메일 주소"
```

#### 주요 옵션:

- `--global`: 시스템 전체에 설정을 적용합니다.
- `--list`: 현재 설정 목록을 확인합니다.

```bash
git config --list
```

---

## 2. Git 저장소 초기화

### **`git init`**

현재 디렉토리를 Git 저장소로 초기화합니다.

```bash
git init
```

명령 실행 후 `.git` 디렉토리가 생성됩니다. 이 디렉토리는 Git의 버전 관리 정보를 포함합니다.

---

## 3. 파일 상태 확인

### **`git status`**

현재 디렉토리의 파일 상태를 확인합니다.

```bash
git status
```

출력:

- **Untracked files**: Git에서 추적하지 않는 새 파일.
- **Changes not staged for commit**: 스테이징되지 않은 변경 사항.
- **Changes to be committed**: 커밋될 준비가 된 파일.

---

## 4. 파일 관리

### **`git add`**

파일을 스테이징 영역에 추가합니다.

```bash
git add 파일명
git add .   # 모든 변경 파일 추가
```

### **`git rm`**

파일을 삭제하거나 스테이징에서 제거합니다.

```bash
git rm 파일명
git rm --cached 파일명  # 스테이징 영역에서만 제거
```

---

## 5. 커밋

### **`git commit`**

변경된 파일을 저장소에 기록합니다.

```bash
git commit -m "커밋 메시지"
git commit  # 에디터에서 메시지 입력
```

### **`git commit --amend`**

최근 커밋 메시지 또는 내용을 수정합니다.

```bash
git commit --amend -m "수정된 메시지"
```

---

## 6. 변경 기록 확인

### **`git log`**

커밋 기록을 확인합니다.

```bash
git log
git log --oneline  # 한 줄 요약
```

### **`git diff`**

변경 내용을 비교합니다.

```bash
git diff         # 작업 디렉토리와 스테이징 영역 비교
git diff --staged  # 스테이징 영역과 마지막 커밋 비교
```

---

## 7. 브랜치 관리

### **`git branch`**

브랜치를 관리합니다.

```bash
git branch        # 브랜치 목록 확인
git branch 새브랜치이름  # 새 브랜치 생성
git branch -d 브랜치이름  # 브랜치 삭제
```

### **`git switch`**

브랜치를 전환하거나 새 브랜치를 생성합니다.

```bash
git switch 브랜치이름
git switch -c 새브랜치이름  # 새 브랜치 생성 및 전환
```

---

## 8. 브랜치 병합

### **`git merge`**

브랜치를 병합합니다.

```bash
git merge 브랜치이름
```

병합 충돌이 발생할 경우 수동으로 수정 후 아래 명령어로 병합을 완료합니다.

```bash
git add .
git commit
```

---

## 9. 원격 저장소 관리

### **`git remote`**

원격 저장소를 추가하거나 확인합니다.

```bash
git remote add origin 원격URL
git remote -v  # 원격 저장소 확인
git remote remove origin # 원격 저장소 연결 해제
```

### **`git clone`**

원격 저장소를 복제합니다.

```bash
git clone 원격URL
```

### **`git push`**

로컬 변경 내용을 원격 저장소에 업로드합니다.

```bash
git push origin 브랜치이름
```

### **`git pull`**

원격 저장소의 변경 내용을 가져오고 병합합니다.

```bash
git pull origin 브랜치이름
```

### **`git fetch`**

원격 저장소의 변경 내용을 가져오지만 병합하지는 않습니다.

```bash
git fetch origin
```

---

## 10. 되돌리기

### **`git reset`**

커밋 또는 스테이징 상태를 되돌립니다.

```bash
git reset HEAD~1  # 최근 커밋 되돌리기
git reset --soft HEAD~1  # 스테이징으로 되돌림
git reset --hard HEAD~1  # 모든 변경 삭제
```

### **`git revert`**

이전 커밋을 되돌리는 새로운 커밋을 생성합니다.

```bash
git revert 커밋ID
```

---

## 11. 태그 관리

### **`git tag`**

특정 커밋에 태그를 추가합니다.

```bash
git tag 태그이름
git tag -a 태그이름 -m "태그 메시지"
git push origin 태그이름  # 태그 원격 저장소로 푸시
```

---

이 문서를 통해 Git의 기본 명령어를 체계적으로 익히고, 실제 프로젝트에서 효율적으로 사용할 수 있길 바랍니다.
