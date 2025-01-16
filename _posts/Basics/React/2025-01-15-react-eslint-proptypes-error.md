---
title: "React ESLint prop-types 에러 해결: PropTypes와 TypeScript 비교"
excerpt: "React에서 ESLint의 prop-types 에러를 해결하는 방법을 PropTypes와 TypeScript를 활용한 두 가지 방식으로 설명합니다. 각각의 장단점을 알아보세요."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, ESLint, PropTypes, TypeScript]
permalink: /react/eslint-proptypes-error/
toc: true
toc_sticky: true
date: 2025-01-15
last_modified_at: 2025-01-15
---

React 프로젝트에서 ESLint가 `prop-types` 관련 에러를 발생시키는 경우, 이는 컴포넌트가 전달받는 props의 유형을 명시적으로 정의하지 않았기 때문입니다. 이를 해결하려면 `PropTypes`를 사용하거나 TypeScript를 도입하는 방법이 있습니다. 이 글에서는 두 가지 방법을 코드 예제와 함께 설명합니다.

## PropTypes를 사용한 해결 방법
`prop-types`는 React에서 props의 유형을 정의하고 검증하기 위한 라이브러리입니다. 아래 단계를 따라 문제를 해결할 수 있습니다.

### 1. PropTypes 설치
먼저 `prop-types`를 설치합니다:
```bash
npm install prop-types
```

### 2. PropTypes 사용 예제
설치 후, 컴포넌트에서 `prop-types`를 사용해 props 유형을 정의합니다:

```jsx
import React from 'react';
import PropTypes from 'prop-types';

const MyComponent = ({ title, count }) => {
  return (
    <div>
      <h1>{title}</h1>
      <p>{count}</p>
    </div>
  );
};

MyComponent.propTypes = {
  title: PropTypes.string.isRequired, // 필수 문자열
  count: PropTypes.number,           // 선택 숫자
};

export default MyComponent;
```

위 코드에서 `title`은 필수 prop이고 문자열이어야 합니다. 반면, `count`는 선택 사항으로 숫자 타입만 허용합니다. `PropTypes`를 사용하면 런타임에서 props의 타입을 검증할 수 있습니다.

---

## TypeScript를 사용한 해결 방법
TypeScript를 사용하면 컴파일 단계에서 props의 타입을 정의하고 검증할 수 있습니다. 이는 `prop-types`보다 강력하고 효율적입니다.

### 1. TypeScript 설치
TypeScript를 프로젝트에 추가하려면 다음 명령어를 실행합니다:
```bash
npm install typescript @types/react @types/react-dom
```

### 2. TypeScript 설정 및 사용 예제
`tsconfig.json` 파일을 생성한 후, 컴포넌트에서 타입을 정의합니다:

```tsx
import React from 'react';

type MyComponentProps = {
  title: string; // 필수 문자열
  count?: number; // 선택 숫자
};

const MyComponent: React.FC<MyComponentProps> = ({ title, count }) => {
  return (
    <div>
      <h1>{title}</h1>
      <p>{count}</p>
    </div>
  );
};

export default MyComponent;
```

위 코드에서 `MyComponentProps` 타입은 `title`과 `count` props의 타입을 정의합니다. `count`는 선택 사항이며, 컴파일 단계에서 타입 검증이 수행됩니다.

---

## PropTypes vs TypeScript
| 특징           | PropTypes                         | TypeScript                       |
|----------------|-----------------------------------|-----------------------------------|
| **검증 시점**   | 런타임                             | 컴파일 타임                       |
| **유형 정의 방법** | `propTypes` 객체 사용              | 타입 또는 인터페이스 사용          |
| **추가 설치 필요** | `prop-types` 라이브러리             | TypeScript 및 관련 타입 패키지      |
| **장점**        | 간단하고 기존 JavaScript 프로젝트에 적합 | 강력한 타입 시스템 제공, 코드 안정성 향상 |

이미 TypeScript를 사용 중이거나 도입할 계획이라면 `prop-types`를 생략해도 무방합니다. 기존 JavaScript 프로젝트에서는 `prop-types`를 활용해 빠르게 문제를 해결할 수 있습니다.

---

이 가이드가 React 프로젝트에서 `prop-types` 관련 에러를 해결하는 데 도움이 되길 바랍니다. 질문이 있다면 댓글로 남겨주세요! 😊

