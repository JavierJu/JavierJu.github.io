---
title: "React props 완벽 가이드: 기본 개념부터 실전 사용법까지"
excerpt: "React에서 props의 개념과 특징을 알아보고, props를 사용한 데이터 전달 방법과 코드 예제를 통해 이를 실전에서 활용하는 방법을 배워봅니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, props, 컴포넌트]
permalink: /react/props-guide/
toc: true
toc_sticky: true
date: 2025-01-14
last_modified_at: 2025-01-14
---

React의 **props**(properties)는 컴포넌트 간 데이터를 전달하기 위해 사용되는 중요한 개념입니다. **props**는 부모 컴포넌트에서 자식 컴포넌트로 전달되며, 자식 컴포넌트는 이를 읽기 전용(read-only)으로 사용합니다. 아래에 props의 주요 특징과 사용법을 자세히 설명하겠습니다.

---

## 1. **props의 주요 특징**

1. **단방향 데이터 흐름**:
   - 데이터는 항상 부모에서 자식으로 전달됩니다.
   - 자식 컴포넌트에서 props를 수정할 수 없습니다.

2. **불변성**:
   - props는 읽기 전용이므로 컴포넌트 내부에서 변경할 수 없습니다.
   - props를 변경하려면 부모 컴포넌트에서 새로운 값을 전달해야 합니다.

3. **동적 데이터 전달**:
   - 부모 컴포넌트의 상태(state)나 다른 데이터를 기반으로 props를 동적으로 전달할 수 있습니다.

4. **구조 분해 할당**:
   - props를 객체로 전달받으므로, 구조 분해 할당을 통해 개별적으로 쉽게 추출할 수 있습니다.

---

## 2. **props 사용 예시**

### 기본 사용법
```jsx
// 부모 컴포넌트
import React from 'react';
import ChildComponent from './ChildComponent';

function ParentComponent() {
  const message = "Hello, React!";
  return <ChildComponent text={message} />;
}

// 자식 컴포넌트
function ChildComponent(props) {
  return <h1>{props.text}</h1>;
}

export default ParentComponent;
```
위 코드에서 `text`라는 props가 부모에서 자식으로 전달되었고, 자식 컴포넌트는 이를 화면에 표시합니다.

---

### 구조 분해 할당 사용
```jsx
function ChildComponent({ text }) {
  return <h1>{text}</h1>;
}
```

---

## 3. **props.children**

React에서는 특별한 props인 `children`이 있습니다. 이는 컴포넌트 태그 사이에 전달된 내용을 나타냅니다.

```jsx
function Wrapper({ children }) {
  return <div className="wrapper">{children}</div>;
}

function App() {
  return (
    <Wrapper>
      <p>This is a child element!</p>
    </Wrapper>
  );
}
```

결과적으로, `Wrapper` 컴포넌트는 `children`으로 `<p>` 요소를 받게 됩니다.

---

## 4. **기본값 설정 (defaultProps)**

컴포넌트에 props가 전달되지 않을 때 기본값을 설정할 수 있습니다.

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

Greeting.defaultProps = {
  name: "Guest",
};
```
만약 `name`이 전달되지 않으면, 기본값 `"Guest"`가 사용됩니다.

---

## 5. **PropTypes로 타입 검사**

React에서는 `prop-types` 라이브러리를 사용해 props의 타입을 검사할 수 있습니다.

```jsx
import PropTypes from 'prop-types';

function Greeting({ name, age }) {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>Your age is {age}.</p>
    </div>
  );
}

Greeting.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number,
};

Greeting.defaultProps = {
  age: 0,
};
```

- `PropTypes.string`은 `name`이 문자열이어야 함을 나타냅니다.
- `isRequired`를 사용하면 필수 prop으로 설정할 수 있습니다.

---

## 6. **props와 state의 차이점**

| 특징         | props                         | state                      |
|--------------|-------------------------------|----------------------------|
| 변경 가능 여부 | 읽기 전용 (Immutable)          | 변경 가능 (Mutable)         |
| 데이터 소스    | 부모 컴포넌트                 | 컴포넌트 자체               |
| 사용 목적      | 컴포넌트 간 데이터 전달         | 동적인 데이터 관리           |

---

props는 React의 핵심 개념 중 하나로, 컴포넌트의 재사용성과 확장성을 높이는 데 매우 유용합니다. 실습을 통해 props의 동작을 이해하면 React 개발에 큰 도움이 될 것입니다. 😊

