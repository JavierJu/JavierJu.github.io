---
title: "NPM depcheck로 프로젝트에서 불필요한 패키지 정리하기"
excerpt: "depcheck는 Node.js 프로젝트에서 사용되지 않는 패키지를 찾아주는 도구입니다. 이 글에서는 depcheck의 기능과 사용법을 자세히 알아보고, 프로젝트에서 불필요한 의존성을 정리하는 방법을 코드 예제와 함께 설명합니다."
categories:
  - Node
  - JavaScript
  - 개발 도구
  - 패키지 관리

tags:
  - [Node.js, JavaScript, NPM, 패키지 관리, depcheck]
permalink: /node/depcheck-guide/
toc: true
toc_sticky: true
date: 2025-02-13
last_modified_at: 2025-02-13
---

## NPM depcheck로 프로젝트에서 불필요한 패키지 정리하기

Node.js 프로젝트를 개발하다 보면 사용하지 않는 패키지가 남아 있는 경우가 많습니다. 프로젝트를 유지보수하면서 이런 불필요한 패키지를 정리하지 않으면 프로젝트가 무거워지고 관리가 어려워질 수 있습니다. `depcheck`는 이러한 문제를 해결해 주는 도구로, 프로젝트에서 사용되지 않는 의존성을 찾아줍니다.

이 글에서는 `depcheck`의 기능과 사용법을 자세히 알아보고, 프로젝트에서 불필요한 패키지를 정리하는 방법을 소개합니다.

## 1. depcheck란?

`depcheck`는 Node.js 프로젝트에서 사용되지 않는 패키지를 탐색하는 오픈 소스 도구입니다. 프로젝트가 커지면서 필요하지 않은 패키지가 남아 있을 가능성이 높은데, `depcheck`를 사용하면 이를 쉽게 정리할 수 있습니다.

### 1.1 depcheck의 주요 기능

✅ 사용되지 않는 `dependencies` 및 `devDependencies` 탐색
✅ 패키지 정리로 프로젝트 최적화
✅ JSON 출력 지원으로 자동화 가능
✅ 특정 디렉토리 및 파일 제외 가능

## 2. depcheck 설치하기

`depcheck`는 글로벌 또는 로컬로 설치할 수 있습니다.

### 2.1 글로벌 설치

전역에서 사용하려면 다음 명령어를 실행합니다:

```sh
npm install -g depcheck
```

### 2.2 프로젝트 로컬 설치

프로젝트 내부에서만 사용하려면:

```sh
npm install --save-dev depcheck
```

## 3. depcheck 사용법

설치 후, 프로젝트의 루트 디렉토리에서 다음 명령어를 실행합니다:

```sh
depcheck
```

이 명령어를 실행하면 현재 프로젝트에서 사용되지 않는 패키지를 출력합니다.

### 3.1 특정 디렉토리에서 실행

```sh
depcheck /path/to/project
```

특정 프로젝트 폴더를 대상으로 분석할 수도 있습니다.

### 3.2 JSON 형식으로 결과 출력

자동화된 스크립트에서 사용하기 위해 JSON 형식으로 출력할 수도 있습니다.

```sh
depcheck --json
```

## 4. depcheck 결과 해석

다음과 같은 결과가 출력될 수 있습니다:

```sh
Unused dependencies
* lodash
* moment

Unused devDependencies
* eslint
* jest
```

여기서:

- `Unused dependencies`: 코드에서 사용되지 않는 일반 패키지 (`dependencies`)
- `Unused devDependencies`: 코드에서 사용되지 않는 개발용 패키지 (`devDependencies`)

이런 패키지들은 아래 명령어로 삭제할 수 있습니다:

```sh
npm uninstall lodash moment eslint jest
```

## 5. depcheck의 한계

`depcheck`는 정적 분석을 수행하기 때문에 다음과 같은 상황에서 정확한 결과를 제공하지 못할 수 있습니다.

### 5.1 동적 `require` 사용

다음과 같이 `require`를 동적으로 사용하면 `depcheck`가 이를 감지하지 못할 수 있습니다.

```js
const moduleName = "lodash";
const _ = require(moduleName);
```

### 5.2 npm 스크립트에서만 사용되는 패키지

패키지가 `package.json`의 `scripts`에서만 사용되는 경우 `depcheck`는 이를 감지하지 못할 수도 있습니다.

```json
"scripts": {
  "lint": "eslint ."
}
```

이 경우 `eslint`가 사용되고 있지만 코드에서 직접 호출되지 않으므로 `depcheck`가 필요 없는 패키지로 오인할 수 있습니다.

## 6. depcheck 옵션

`depcheck`는 다양한 옵션을 지원하여 분석을 더 세밀하게 조정할 수 있습니다.

### 6.1 특정 폴더 또는 파일 제외

```sh
depcheck --ignores="test,config.js"
```

위 명령어를 사용하면 `test` 폴더와 `config.js` 파일을 분석에서 제외합니다.

### 6.2 특정 패키지를 무시

자동으로 감지되지 않는 ESLint, Jest 등의 패키지를 무시할 수도 있습니다.

```sh
depcheck --ignore-matches="eslint,jest"
```

## 7. depcheck와 함께 사용하면 좋은 도구들

- `npm prune` → `node_modules`에 남아 있는 불필요한 패키지 제거
  ```sh
  npm prune
  ```
- `npx npkill` → 사용하지 않는 `node_modules` 폴더 삭제
  ```sh
  npx npkill
  ```
- `npm audit fix` → 보안 취약점이 있는 패키지 업데이트
  ```sh
  npm audit fix
  ```

## 8. 결론

`depcheck`는 프로젝트에서 불필요한 패키지를 찾고 정리하는 데 매우 유용한 도구입니다. 하지만 동적 `require` 사용이나 npm 스크립트 내 사용을 정확히 감지하지 못하는 경우도 있으므로, 결과를 검토한 후 삭제하는 것이 좋습니다.

정기적으로 `depcheck`를 실행하면 프로젝트의 크기를 줄이고 유지보수를 쉽게 할 수 있습니다. 🚀
