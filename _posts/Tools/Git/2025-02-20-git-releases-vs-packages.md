---
title: "GitHub Releases와 Packages: 차이점과 활용 방법"
excerpt: "GitHub의 Releases와 Packages 기능을 비교하고, NPM 패키지 및 Docker 컨테이너 배포를 위한 설정 방법을 코드 예제와 함께 설명합니다."
categories:
  - GitHub
  - Git
  - DevOps
  - CI/CD
  - Package Management
tags:
  - [GitHub, Releases, Packages, NPM, Docker, DevOps, CI/CD]
permalink: /git/releases-vs-packages/
toc: true
toc_sticky: true
date: 2025-02-20
last_modified_at: 2025-02-20
---

## GitHub Releases와 Packages 차이점 및 활용 방법

GitHub의 **Releases**와 **Packages**는 각각 배포 및 패키지 관리를 위한 기능입니다.

---

## 🔹 GitHub Releases

GitHub의 **Releases(릴리스)** 는 특정 커밋에 태그를 지정하여 소프트웨어의 버전(예: v1.0.0, v2.0.0)으로 배포하는 기능입니다.

### ✅ 주요 기능

1. **태그(Tag)와 연결**

   - 릴리스는 Git의 태그와 연결됩니다. (예: `v1.0.0`, `v1.1.0-beta`)
   - 태그를 통해 특정 커밋의 상태를 유지할 수 있습니다.

2. **릴리스 노트 작성 가능**

   - 변경 사항, 새로운 기능, 버그 수정 내역 등을 기록할 수 있습니다.

3. **바이너리 파일 업로드 가능**
   - 실행 파일(`.zip`, `.exe`, `.tar.gz` 등)을 업로드할 수 있습니다.

### 🛠 설정 방법

1. GitHub 저장소에서 **Releases** 탭으로 이동.
2. **"Draft a new release"** 클릭.
3. **새 태그(Tag) 생성** (예: `v1.0.0`)
4. 릴리스 제목 및 설명 추가.
5. (선택) 실행 파일 또는 패키지를 업로드.
6. "Publish release" 클릭하여 배포 완료.

👉 **사용 예시:**

- 특정 버전의 코드 또는 빌드 파일 배포.
- 사용자가 직접 실행할 수 있는 실행 파일 제공.

---

## 🔹 GitHub Packages

GitHub Packages는 **NPM, Docker, Maven, Gradle** 같은 패키지 관리자와 연동되는 **패키지 레지스트리** 기능입니다.

### ✅ 주요 기능

1. **NPM, Docker 등의 패키지를 저장소에 직접 배포**

   - 예: `npm publish`로 GitHub에 NPM 패키지 업로드.

2. **Private 및 Public 패키지 지원**

   - 기본적으로 GitHub 계정과 연동된 패키지 관리 가능.

3. **GitHub Actions와 연동 가능**
   - CI/CD 파이프라인에서 자동 배포 가능.

---

## 📦 GitHub Packages 활용 방법

### ✅ 1. NPM 패키지 배포 및 사용

GitHub Packages를 **NPM 패키지 레지스트리**로 사용하면, 특정 기능을 패키지로 만들어 팀원들과 공유하거나 개인 프로젝트에서 재사용할 수 있습니다.

### **🛠 설정 방법 (NPM 패키지 배포)**

1️⃣ **GitHub에 Personal Access Token 발급**

- [GitHub Personal Access Token (PAT)](https://github.com/settings/tokens)에서 `write:packages`, `read:packages`, `delete:packages` 권한이 있는 토큰을 생성합니다.

2️⃣ **프로젝트 폴더에서 `package.json` 설정**

```json
{
  "name": "@your-github-username/your-package-name",
  "version": "1.0.0",
  "publishConfig": {
    "registry": "https://npm.pkg.github.com/"
  }
}
```

3️⃣ **`.npmrc` 파일 생성 (토큰 입력)**

```sh
echo "//npm.pkg.github.com/:_authToken=YOUR_GITHUB_TOKEN" > ~/.npmrc
```

4️⃣ **NPM 패키지 배포**

```sh
npm publish
```

5️⃣ **다른 프로젝트에서 설치하여 사용**

```sh
npm install @your-github-username/your-package-name
```

👉 **활용 예시:**

- 자주 사용하는 유틸리티 함수 패키지 (`@yourname/utils`)
- 내부적으로만 사용하는 UI 컴포넌트 라이브러리 (`@yourname/ui-library`)

---

### ✅ 2. Docker 컨테이너 이미지 배포

GitHub Packages는 **Docker 이미지 저장소 역할**도 수행할 수 있습니다.  
개발 환경을 통일하거나 배포 프로세스를 자동화할 때 유용합니다.

### **🛠 설정 방법 (Docker 이미지 배포)**

1️⃣ **GitHub 로그인 후 Docker 빌드**

```sh
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin
```

2️⃣ **Dockerfile 작성**

```Dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```

3️⃣ **이미지 태깅**

```sh
docker tag my-app ghcr.io/your-github-username/my-app:latest
```

4️⃣ **Docker 이미지 업로드**

```sh
docker push ghcr.io/your-github-username/my-app:latest
```

5️⃣ **다른 환경에서 컨테이너 실행**

```sh
docker run ghcr.io/your-github-username/my-app:latest
```

👉 **활용 예시:**

- 팀 내부에서 동일한 개발 환경을 배포하는 용도.
- 프로젝트의 공식 컨테이너 이미지 배포.

---

## 🏁 정리

| 활용 사례                   | 기술           | 활용 방법                      |
| --------------------------- | -------------- | ------------------------------ |
| 🔹 **공개 NPM 패키지 배포** | NPM            | `npm publish` 후 `npm install` |
| 🔹 **사내 전용 라이브러리** | NPM            | Private 패키지 설정 후 공유    |
| 🔹 **Docker 컨테이너 배포** | Docker         | GitHub Container Registry 사용 |
| 🔹 **팀 내 공통 환경 관리** | Docker         | 공통 이미지 배포 및 실행       |
| 🔹 **CI/CD 자동 배포**      | GitHub Actions | 패키지/컨테이너 자동 업데이트  |

💡 **GitHub Packages는 코드 공유 및 배포를 간소화하는 강력한 도구!**  
내부 라이브러리를 체계적으로 관리하거나, 배포 자동화를 고려할 때 활용하면 좋습니다. 🚀
