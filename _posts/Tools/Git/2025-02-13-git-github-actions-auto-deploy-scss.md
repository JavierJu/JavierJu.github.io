---
title: "GitHub Actions 자동 배포 및 SCSS 변경 사항 감지 후 자동 푸시"
excerpt: "GitHub Actions를 활용하여 SCSS 파일 변경 사항을 감지하고 자동으로 style.css를 빌드 및 푸시하는 방법을 설명합니다. CI/CD 자동화를 통해 효율적인 개발 환경을 구축해 보세요."
categories:
  - GitHub Actions
  - CI/CD
  - Frontend
  - SASS
  - Automation
  - Git

tags:
  - [GitHub Actions, CI/CD, SCSS, 자동화, Git, 배포]
permalink: /git/github-actions-auto-deploy-scss/
toc: true
toc_sticky: true
date: 2025-02-13
last_modified_at: 2025-02-13
---

## GitHub Actions 자동 배포 및 SCSS 변경 사항 감지 후 자동 푸시

## 📌 개요

GitHub Actions를 사용하여 **SCSS 파일 변경을 감지하고, `style.css`를 자동 빌드한 후, 변경된 파일을 자동으로 커밋 & 푸시**하는 CI/CD 프로세스를 구축합니다.

이 설정을 통해 개발자는 SCSS 파일을 변경한 후 **직접 `npm run build`를 실행하고 커밋할 필요 없이**, GitHub Actions가 자동으로 처리해 줍니다.

## 🚀 자동화 목표

1. **SCSS 파일 변경 감지** → `git diff`를 사용하여 변경 사항 확인
2. **SCSS 빌드 실행** → `npm run build`를 실행하여 `style.css` 생성
3. **자동 커밋 및 푸시** → 변경된 `style.css`와 `style.css.map`을 GitHub에 자동 푸시

---

## 🛠 GitHub Actions 설정

### 1️⃣ **GitHub Actions YAML 파일 생성**

`.github/workflows/deploy.yml` 파일을 생성하고 다음 내용을 추가합니다.

```yaml
name: Deploy to S3 and Auto Push style.css

on:
  push:
    branches:
      - main # main 브랜치에 푸시하면 자동 배포

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
        with:
          fetch-depth: 0 # 모든 커밋을 가져와 비교할 수 있도록 설정

      - name: Check if SASS Files Changed
        id: sass_check
        run: |
          if git diff --name-only --diff-filter=AMR HEAD^ HEAD | grep -E 'sass/.*\.scss$'; then
            echo "sass_changed=true" >> $GITHUB_ENV
          else
            echo "sass_changed=false" >> $GITHUB_ENV
          fi

      - name: Build Project (Generate style.css)
        if: env.sass_changed == 'true'
        run: npm run build

      - name: Commit and Push Changes (style.css & style.css.map)
        if: env.sass_changed == 'true'
        run: |
          git config --global user.name "github-actions[bot]"
          git config --global user.email "github-actions[bot]@users.noreply.github.com"

          # Git 상태 확인 (디버깅용)
          git status
          git diff --staged

          # 변경 사항 추가 및 커밋
          git add -A
          git commit -m "Auto-generate style.css and style.css.map from updated SASS files" --allow-empty
          git push origin main

      - name: Deploy to S3
        run: aws s3 sync public/ s3://www.example.com --delete

      - name: Invalidate CloudFront Cache
        run: aws cloudfront create-invalidation --distribution-id YOUR_DISTRIBUTION_ID --paths "/*"
```

---

## 📝 **GitHub Actions 코드 설명**

### 1️⃣ **SCSS 변경 감지** (`git diff` 사용)

```yaml
if git diff --name-only --diff-filter=AMR HEAD^ HEAD | grep -E 'sass/.*\.scss$'; then
echo "sass_changed=true" >> $GITHUB_ENV
else
echo "sass_changed=false" >> $GITHUB_ENV
fi
```

- `git diff --name-only --diff-filter=AMR HEAD^ HEAD` → 마지막 커밋과 이전 커밋을 비교하여 **변경된 SCSS 파일 목록을 가져옴**.
- `grep -E 'sass/.*\.scss$'` → `sass/` 디렉터리 내의 `.scss` 파일이 변경되었는지 확인.
- 변경이 감지되면 `sass_changed=true` 값을 GitHub Actions 환경 변수(`$GITHUB_ENV`)에 저장.

### 2️⃣ **SCSS 빌드 실행**

```yaml
if: env.sass_changed == 'true'
run: npm run build
```

- SCSS 파일이 변경된 경우에만 `npm run build` 실행 → **불필요한 빌드 방지**.

### 3️⃣ **자동 커밋 & 푸시**

```yaml
git add -A
git commit -m "Auto-generate style.css and style.css.map from updated SASS files" --allow-empty
git push origin main
```

- `git add -A` → 변경된 모든 파일 추가 (`style.css`, `style.css.map` 포함).
- `git commit -m "Auto-generate style.css and style.css.map from updated SASS files" --allow-empty` → 변경 사항이 없어도 빈 커밋을 생성.
- `git push origin main` → 변경 사항을 원격 저장소에 푸시.

### 4️⃣ **AWS S3 배포 및 CloudFront 캐시 무효화**

```yaml
aws s3 sync public/ s3://www.example.com --delete
aws cloudfront create-invalidation --distribution-id YOUR_DISTRIBUTION_ID --paths "/*"
```

- 최신 파일을 **AWS S3에 배포**하고, **CloudFront 캐시를 무효화하여 변경 사항 즉시 반영**.

---

## 🎯 **결론 및 기대 효과**

✅ **SCSS 변경 시 자동 감지 → 자동 빌드 & 자동 푸시**로 배포 자동화 구현!  
✅ **불필요한 빌드 방지** → SCSS 변경 사항이 없으면 `npm run build` 실행 안 함.  
✅ **CloudFront 캐시 무효화** → 배포 후 즉시 새로운 스타일 반영 가능!

이제 GitHub Actions를 활용하여 **더 효율적인 CI/CD 파이프라인을 구축하세요!** 🚀
