---
title: "Node.js bcrypt 모듈 완벽 가이드: 비밀번호 보안을 강화하자"
excerpt: "bcrypt 모듈의 개념, 설치 방법, 사용법, 그리고 보안 관련 권장사항을 다루며 Node.js 애플리케이션에서 비밀번호 관리의 최적화를 제공합니다."
categories:
  - Node
  - Security
tags:
  - [JavaScript, Node.js, Backend, Security, bcrypt]
permalink: /node/bcrypt-guide/
toc: true
toc_sticky: true
date: 2024-12-31
last_modified_at: 2024-12-31
---

## bcrypt란 무엇인가?

`bcrypt`는 Node.js 애플리케이션에서 비밀번호를 안전하게 해시하고 검증하기 위해 자주 사용되는 라이브러리입니다. 비밀번호를 평문으로 저장하지 않고, 안전한 해시 알고리즘을 사용하여 암호화된 형태로 저장하도록 도와줍니다. 이는 보안성을 높이는 데 중요한 역할을 합니다.

---

## 주요 특징

1. **단방향 해시**:
   - `bcrypt`는 비밀번호를 단방향으로 해시합니다. 해시된 결과에서 원래 비밀번호를 복구할 수 없습니다.
   - 해시된 비밀번호와 사용자가 입력한 비밀번호를 비교하는 방식으로 인증합니다.

2. **솔트(Salt)**:
   - 솔트는 해시 계산 전에 비밀번호에 추가되는 임의의 값입니다.
   - 같은 비밀번호라도 서로 다른 해시값을 생성하여 레인보우 테이블 공격을 방지합니다.

3. **워크 팩터(Work Factor)**:
   - 해싱 과정에서의 반복 횟수를 설정할 수 있습니다. 이 반복 횟수를 "Salt Rounds"라고 하며, 숫자가 클수록 더 많은 연산이 필요합니다.
   - 워크 팩터가 클수록 보안성은 증가하지만, 처리 속도는 느려집니다.

4. **강력한 알고리즘**:
   - `bcrypt`는 Blowfish 암호를 기반으로 한 알고리즘을 사용하여, 기존의 MD5, SHA와 비교해 더 안전한 보안성을 제공합니다.

---

## 설치

`bcrypt`는 두 가지 버전으로 제공됩니다:
- **`bcrypt` (C++ 기반 네이티브 모듈)**
- **`bcryptjs` (순수 JavaScript로 구현)**

### 설치 방법

#### 1. 네이티브 `bcrypt` 설치
```bash
npm install bcrypt
```

#### 2. 순수 JavaScript `bcryptjs` 설치
```bash
npm install bcryptjs
```
> 참고: `bcryptjs`는 네이티브 모듈 설치가 어려운 환경(예: Windows 또는 특정 CI/CD 환경)에서 유용합니다.

---

## 기본 사용법

### 1. 비밀번호 해싱

#### bcrypt (Native)
```javascript
const bcrypt = require('bcrypt');

const saltRounds = 10; // 솔트 라운드 설정
const password = 'mySecretPassword';

bcrypt.hash(password, saltRounds, (err, hash) => {
  if (err) throw err;
  console.log('Hashed Password:', hash);
});
```

#### bcryptjs
```javascript
const bcrypt = require('bcryptjs');

const saltRounds = 10; // 솔트 라운드 설정
const password = 'mySecretPassword';

bcrypt.hash(password, saltRounds, (err, hash) => {
  if (err) throw err;
  console.log('Hashed Password:', hash);
});
```

---

### 2. 비밀번호 검증

#### bcrypt (Native)
```javascript
const bcrypt = require('bcrypt');

const storedHash = '$2b$10$abcdef...'; // 데이터베이스에 저장된 해시
const inputPassword = 'mySecretPassword';

bcrypt.compare(inputPassword, storedHash, (err, isMatch) => {
  if (err) throw err;
  console.log('Password Match:', isMatch); // true or false
});
```

#### bcryptjs
```javascript
const bcrypt = require('bcryptjs');

const storedHash = '$2b$10$abcdef...'; // 데이터베이스에 저장된 해시
const inputPassword = 'mySecretPassword';

bcrypt.compare(inputPassword, storedHash, (err, isMatch) => {
  if (err) throw err;
  console.log('Password Match:', isMatch); // true or false
});
```

---

### 3. 비동기/동기 방식

`bcrypt`와 `bcryptjs`는 모두 비동기 및 동기 API를 제공합니다.

#### 비동기 방식 (권장)
- 성능 및 응답성을 유지하기 위해 비동기 사용이 권장됩니다.
```javascript
bcrypt.hash(password, saltRounds, (err, hash) => { ... });
bcrypt.compare(inputPassword, storedHash, (err, isMatch) => { ... });
```

#### 동기 방식
- 간단한 테스트나 CLI 환경에서 유용하지만, 서버의 응답성을 저하시킬 수 있습니다.
```javascript
const hash = bcrypt.hashSync(password, saltRounds);
const isMatch = bcrypt.compareSync(inputPassword, storedHash);
```

---

## 주요 옵션 및 메서드

1. **`bcrypt.hash`**:
   - 비밀번호를 해시합니다.
   - 인자: `password`, `saltRounds`, `callback`
   - 반환값: `hashed password`

2. **`bcrypt.compare`**:
   - 입력 비밀번호와 저장된 해시를 비교합니다.
   - 인자: `inputPassword`, `storedHash`, `callback`
   - 반환값: `boolean` (`true` 또는 `false`)

3. **`bcrypt.genSalt`**:
   - 해싱에 사용되는 솔트를 생성합니다.
   - 인자: `saltRounds`, `callback`
   - 반환값: `salt`

4. **`bcrypt.getRounds`**:
   - 해시에서 사용된 솔트 라운드를 확인합니다.
   ```javascript
   const rounds = bcrypt.getRounds(storedHash);
   console.log('Rounds:', rounds);
   ```

---

## 보안 관련 권장사항

1. **적절한 솔트 라운드 설정**:
   - 솔트 라운드는 성능과 보안성 사이의 균형을 고려하여 설정해야 합니다.
   - 일반적으로 `10 ~ 12`가 적절하지만, 서버 성능에 따라 조정할 수 있습니다.

2. **비밀번호 정책 적용**:
   - 사용자가 쉽게 추측할 수 없는 강력한 비밀번호를 사용하도록 요구하세요.

3. **정기적인 해시 업데이트**:
   - 시스템 성능이 개선되면 기존에 저장된 해시를 더 강력한 솔트 라운드로 재해싱하세요.

---

`bcrypt`는 비밀번호 보안을 강화하는 데 매우 유용하며, Express 같은 웹 프레임워크와 함께 자주 사용됩니다. 더 자세한 구현이나 상황별 사용법이 필요하다면 언제든 질문해주세요! 😊

