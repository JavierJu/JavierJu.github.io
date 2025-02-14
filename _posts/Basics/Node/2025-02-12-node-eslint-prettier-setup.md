---
title: "Express.js 프로젝트에서 ESLint와 Prettier 설정하는 방법"
excerpt: "Express.js 기반 프로젝트에서 ESLint와 Prettier를 설정하여 코드 품질과 일관성을 유지하는 방법을 단계별로 설명합니다. Flat Config 방식과 최신 ESLint 버전에 맞춘 설정법을 코드 예제와 함께 제공합니다."
categories:
  - Express
  - Backend
  - ESLint
  - Prettier
  - Node
tags:
  - [Express.js, JavaScript, ESLint, Prettier, 코드 품질, Backend]
permalink: /node/eslint-prettier-setup/
toc: true
toc_sticky: true
date: 2025-02-12
last_modified_at: 2025-02-12
---

Express.js 프로젝트에서 코드 품질을 유지하고 일관된 코드 스타일을 적용하려면 **ESLint**와 **Prettier**를 함께 설정하는 것이 중요합니다. 이 가이드는 최신 ESLint(Flat Config 방식)와 Prettier를 Express.js 프로젝트에 적용하는 방법을 단계별로 설명합니다.

---

## 1. ESLint 및 Prettier 설치

터미널을 열고 프로젝트 루트에서 다음 명령어를 실행하세요:

```bash
npm install eslint prettier eslint-config-prettier eslint-plugin-node --save-dev
```

설치되는 패키지 설명:
- **eslint**: 코드 품질 분석 도구
- **prettier**: 코드 포매터 (코드 스타일 정리)
- **eslint-config-prettier**: ESLint와 Prettier 간 충돌을 방지하는 설정
- **eslint-plugin-node**: Node.js 코드 규칙을 위한 ESLint 플러그인

---

## 2. ESLint 설정 (`eslint.config.mjs` 생성)

ESLint 최신 버전(9.x 이상)에서는 **Flat Config 방식**을 사용합니다. 기존 `.eslintrc` 파일 대신 **`eslint.config.mjs`** 파일을 생성해야 합니다.

### ✅ `eslint.config.mjs` 설정 예제

```javascript
import globals from "globals";
import pluginJs from "@eslint/js";
import pluginNode from "eslint-plugin-node";
import prettier from "eslint-config-prettier";

/** @type {import('eslint').Linter.FlatConfigData[]} */
export default [
  {
    files: ["**/*.js"],
    languageOptions: {
      sourceType: "commonjs",
      globals: globals.node,
    },
    plugins: {
      node: pluginNode,
    },
    rules: {
      "no-console": "off",
      "no-unused-vars": ["warn"],
      "eqeqeq": ["error", "always"],
      "curly": ["error", "all"],
      "consistent-return": "off",
    },
  },
  pluginJs.configs.recommended,
  prettier,
];
```

### 🔍 주요 설정 설명
- **`globals.node`**: Node.js의 전역 객체(`require`, `module`, `__dirname` 등) 허용
- **`no-console": "off"`**: `console.log` 사용 가능하도록 설정
- **`no-unused-vars": "warn"`**: 사용되지 않는 변수는 경고 표시
- **`eqeqeq": "error"`**: `==` 대신 `===` 강제
- **`curly": "error"`**: 모든 `if`, `else`에 중괄호 `{}` 사용 강제

---

## 3. Prettier 설정 (`.prettierrc` 생성)

프로젝트 루트에 `.prettierrc` 파일을 만들고 다음 내용을 추가하세요:

```json
{
  "semi": true,
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

### 📌 설명
- **`semi: true`** → 세미콜론(;)을 사용
- **`singleQuote: true`** → 작은 따옴표(`'`) 사용
- **`printWidth: 80`** → 한 줄 길이를 80자로 제한
- **`trailingComma: "es5"`** → ES5 문법에 맞게 마지막 쉼표 유지

---

## 4. ESLint 및 Prettier 무시 파일 설정

### ✅ `.eslintignore`
```bash
node_modules/
```

### ✅ `.prettierignore`
```bash
node_modules/
```

이렇게 하면 `node_modules/` 내부 파일을 검사하지 않아 불필요한 오류를 방지할 수 있습니다.

---

## 5. `package.json`에 실행 스크립트 추가

`package.json`에 다음과 같은 스크립트를 추가하면 ESLint와 Prettier를 쉽게 실행할 수 있습니다.

```json
"scripts": {
  "lint": "eslint .",
  "lint:fix": "eslint . --fix",
  "format": "prettier --write ."
}
```

### 🛠 실행 명령어
- **`npm run lint`** → 코드 검사 수행
- **`npm run lint:fix`** → 자동으로 수정 가능한 문제 해결
- **`npm run format`** → 코드 스타일을 Prettier로 정리

---

## 6. 실행 및 확인

설정이 완료되었으면, 터미널에서 다음 명령어를 실행해 정상적으로 작동하는지 확인하세요:

```bash
npm run lint
npm run lint:fix
npm run format
```

**성공적으로 실행되면 ESLint와 Prettier가 정상적으로 적용된 것입니다!** 🎉

---

## 마무리

Express.js 프로젝트에서 ESLint와 Prettier를 설정하면 **코드 품질을 유지하고 일관된 스타일을 적용하는 데** 큰 도움이 됩니다. 최신 Flat Config 방식에 맞춰 설정하였으므로, 최신 ESLint 버전에서도 안정적으로 작동합니다.

**🚀 추가 질문이 있다면 댓글로 남겨주세요!**

