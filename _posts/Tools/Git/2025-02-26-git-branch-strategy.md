---
title: "Git 브랜치 전략: 프로덕션 환경에서의 분기, 병합, 배포 및 버전 관리"
excerpt: "Git 브랜치 전략을 활용하여 실제 개발 및 프로덕션 환경에서 효과적으로 기능을 개발하고, 통합, 테스트, 배포하는 방법을 코드 예제와 함께 설명합니다."
categories:
  - Git
  - Version Control
  - DevOps
  - Workflow

tags:
  - [Git, 브랜치 전략, 버전 관리, DevOps, 소프트웨어 개발]
permalink: /git/branch-strategy/
toc: true
toc_sticky: true
date: 2025-02-26
last_modified_at: 2025-02-26
---

실제 개발 및 프로덕션 환경에서 Git 브랜치를 어떻게 분기하고 관리하는지에 대해 실질적인 예시를 들어 설명하겠습니다. 가장 널리 사용되는 Git 브랜치 전략 중 하나인 **Git Flow** 기반으로 설명합니다.

---

## **1️⃣ 초기 개발 (Initial Development)**

처음 프로젝트를 시작하면 **main** 브랜치와 **develop** 브랜치를 생성합니다.

```sh
git init
git checkout -b main
git checkout -b develop
```

- **main**: 실제 운영되는(배포되는) 브랜치
- **develop**: 개발을 진행하는 브랜치

---

## **2️⃣ 기능 개발 (Feature Development)**

새로운 기능을 추가할 때는 **feature 브랜치**를 만들어 개발합니다.

```sh
git checkout -b feature/login
# 로그인 기능 개발 후 커밋
git commit -m "✨ [feature] 로그인 기능 추가"
git push origin feature/login
```

개발이 완료되면 **develop 브랜치로 병합(merge)** 합니다.

```sh
git checkout develop
git merge feature/login --no-ff
git push origin develop
```

💡 `--no-ff` 옵션을 사용하면 병합 기록이 남아 추적이 쉬워집니다.

---

## **3️⃣ 통합 및 테스트 (Integration & Testing)**

여러 기능이 develop 브랜치로 합쳐졌다면, QA 및 테스트를 위해 **release 브랜치**를 만듭니다.

```sh
git checkout -b release/v1.0
# 테스트 중 발견된 버그 수정 후 커밋
git commit -m "🐛 [fix] 로그인 버그 수정"
git push origin release/v1.0
```

---

## **4️⃣ 배포 (Production Release)**

배포가 확정되면 **main 브랜치에 병합하고 버전 태그(tag) 추가** 후, 배포합니다.

```sh
git checkout main
git merge release/v1.0 --no-ff
git tag -a v1.0 -m "🎉 Release v1.0"
git push origin main --tags
```

이제 배포 완료! 🚀

하지만 배포 후에도 유지보수는 계속됩니다.

---

## **5️⃣ 버그 수정 (Hotfix)**

배포된 후 긴급한 버그가 발견되면 **hotfix 브랜치를 생성하여 빠르게 수정**합니다.

```sh
git checkout -b hotfix/login-bug
# 긴급 수정 후 커밋
git commit -m "🚑 [hotfix] 로그인 시 비밀번호 검증 오류 수정"
git push origin hotfix/login-bug
```

수정된 내용을 **main**과 **develop**에 병합합니다.

```sh
git checkout main
git merge hotfix/login-bug --no-ff
git tag -a v1.0.1 -m "🚑 Hotfix v1.0.1"
git push origin main --tags

git checkout develop
git merge hotfix/login-bug --no-ff
git push origin develop
```

---

## **6️⃣ 새 기능 추가 & 버전 업**

기능 추가가 계속되면서 `v1.1` 버전을 준비한다고 가정합니다.

```sh
git checkout -b feature/profile-update
# 프로필 업데이트 기능 개발
git commit -m "✨ [feature] 프로필 수정 기능 추가"
git push origin feature/profile-update
```

위와 같은 방식으로 기능을 추가하고, 다시 `release/v1.1` 브랜치를 만들어 QA → main 병합 → 태그 추가 → 배포 과정을 반복합니다.

---

## **✅ 최종 브랜치 구조**

```
main  ────────────●─────●───────────────●──────●────
                  │     │               │      │
develop ────────●│─────●───────────────●──────●────
                ││     │               │      │
feature/login  ●│     │
                │     │
feature/profile-update ●
                │
release/v1.0  ●───────────────────────────────
                │
release/v1.1  ●───────────────────────────────
                │
hotfix/login-bug ●──●─────────────────────────
```

---

## **📌 Commit 메시지 예시**

✔ 좋은 커밋 메시지 예시는 다음과 같습니다.

| 타입          | 메시지                                            |
| ------------- | ------------------------------------------------- |
| ✨ `feature`  | "✨ [feature] 로그인 기능 추가"                   |
| 🐛 `fix`      | "🐛 [fix] 로그인 시 비밀번호 검증 오류 수정"      |
| 🚑 `hotfix`   | "🚑 [hotfix] 긴급 로그인 버그 수정"               |
| ♻️ `refactor` | "♻️ [refactor] 코드 리팩토링 및 변수 네이밍 변경" |
| 📦 `chore`    | "📦 [chore] ESLint 설정 추가"                     |
| 📝 `docs`     | "📝 [docs] README 업데이트"                       |

---

이제 전체적인 Git 브랜치 전략이 정리되었습니다! 🎯  
이 방식을 프로젝트에 적용하면 **협업, 배포, 버전 관리가 체계적으로 운영**될 수 있습니다. 🚀
