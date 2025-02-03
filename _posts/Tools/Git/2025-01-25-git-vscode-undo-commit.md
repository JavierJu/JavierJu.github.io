---
title: "VS Code에서 Commit 취소하기: 실수된 커밋을 되돌리는 방법"
excerpt: "VS Code에서 실수로 커밋한 변경 사항을 되돌리는 다양한 방법을 코드 예제와 함께 설명합니다."
categories:
  - Git
  - VS Code
  - 개발 도구
  - 버전 관리
  - 오류 해결
  
tags:
  - [Git, VS Code, Commit, 취소, 버전 관리]
permalink: /git/vscode-undo-commit/
toc: true
toc_sticky: true
date: 2025-01-25
last_modified_at: 2025-01-25
---

## VS Code에서 Commit 취소하기

코드 작업 중 실수로 커밋(Commit)한 경우, Push(동기화)를 하지 않았다면 비교적 간단하게 커밋을 되돌릴 수 있습니다. VS Code와 Git 명령어를 활용하여 커밋을 취소하는 방법을 단계별로 살펴보겠습니다.

---

### 1. 마지막 커밋 취소 (Soft Reset)

마지막으로 커밋한 변경 사항을 **취소**하고, 변경 내용을 **Staging Area**에 유지하려면 아래 방법을 사용하세요.

#### 터미널 명령어:
```bash
git reset --soft HEAD~1
```

- **`--soft` 옵션**: 마지막 커밋만 취소하며, 변경 사항은 **Staging Area**(Git Changes)에 그대로 남습니다.
- **취소 후 상태**:
  - 변경 파일들은 여전히 Staging Area에 유지됩니다.
  - 새로운 커밋을 생성하거나, 변경 내용을 수정할 수 있습니다.

---

### 2. 커밋 취소 및 Staging Area에서도 제거 (Mixed Reset)

변경 내용을 Staging Area에서 제거하고, **Working Directory(작업 디렉토리)**에만 남기고 싶다면 아래 명령어를 사용하세요.

#### 터미널 명령어:
```bash
git reset HEAD~1
```

- **`--mixed` 옵션 (기본값)**: Staging Area에서 변경 내용을 제거하지만, 파일은 그대로 남아 있습니다.
- **취소 후 상태**:
  - 변경 파일은 Working Directory에 남습니다.
  - 다시 Staging Area로 추가하거나 수정 후 커밋할 수 있습니다.

---

### 3. 특정 커밋만 되돌리기 (Revert)

여러 커밋 중 특정 커밋을 되돌리려면 `git revert` 명령어를 사용합니다. 이는 이전 커밋을 취소하는 **새로운 커밋**을 생성합니다.

#### 터미널 명령어:
```bash
git log
```
위 명령어로 원하는 커밋의 해시 값을 확인하고 복사합니다.

```bash
git revert <커밋 해시>
```

- **`--no-commit` 옵션**: 변경 사항을 Staging Area에만 유지하고 바로 커밋하지 않습니다.

```bash
git revert --no-commit <커밋 해시>
```

---

### 4. VS Code UI에서 Commit 취소하기

VS Code의 UI를 통해서도 간단히 커밋을 취소할 수 있습니다.

#### 방법:
1. **소스 제어(Git Changes)** 패널을 엽니다.
2. 상단의 `...` (더보기) 메뉴를 클릭합니다.
3. **"Undo Last Commit"** 옵션을 선택합니다.
4. 변경 파일이 다시 Staging Area로 복원됩니다.

---

### 주의사항

- **Push 전**: 위 작업은 모두 로컬 커밋만 취소하며, 원격 리포지토리에 영향을 주지 않습니다.
- **Push 후 취소**: 원격 저장소에 Push한 커밋을 취소하려면 추가 작업이 필요합니다 (예: `git push --force`). 이를 사용할 때는 팀원과의 협업에 주의하세요.

---

실수로 커밋했더라도 위 방법을 사용하면 간단히 되돌릴 수 있습니다. 상황에 맞는 방법을 선택하여 효과적으로 작업을 이어가세요!

