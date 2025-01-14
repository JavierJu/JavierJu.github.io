---
title: "React 컴포넌트 이름 대소문자 규칙: 문제와 해결 방법"
excerpt: "React 컴포넌트 이름 대소문자 규칙의 중요성을 알아보고, 잘못된 컴포넌트 이름으로 인해 발생하는 문제를 해결하는 방법을 코드 예제와 함께 설명합니다."
categories:
  - React
  - Frontend
  - JavaScript
tags:
  - [React, JavaScript, Frontend, 컴포넌트, 오류 해결]
permalink: /react/component-naming-rules/
toc: true
toc_sticky: true
date: 2025-01-13
last_modified_at: 2025-01-13
---

## React 컴포넌트 이름 대소문자 규칙: 문제와 해결 방법

React에서 컴포넌트를 작성할 때는 반드시 이름을 대문자로 시작해야 합니다. 이름을 소문자로 시작하면 React는 해당 태그를 HTML 기본 태그로 간주하기 때문에 예상치 못한 오류가 발생할 수 있습니다.

### 문제 상황
다음과 같은 코드를 작성했다고 가정해봅시다.

#### `app.js`
```jsx
import "./styles.css";
import Greeter from "./Greeter";
import loginForm from "./loginForm";
import { Dog, add } from "./Dog";

add(1, 2);

export default function App() {
  return (
    <div className="App">
      <loginForm />
      <Greeter />
      <Dog />
    </div>
  );
}
```

#### `loginForm.js`
```jsx
function loginForm() {
  return (
    <form>
      <input type="text" name="username" id="username" />
      <input type="password" name="password" id="password" />
    </form>
  );
}

export default loginForm;
```

이 코드를 실행하면 `loginForm` 컴포넌트가 화면에 렌더링되지 않는 문제가 발생합니다. 이는 React가 `<loginForm />`을 컴포넌트로 인식하지 못하고 HTML 요소로 처리하기 때문입니다.

### 원인 분석
React에서는 컴포넌트 이름이 **대문자로 시작해야** 컴포넌트로 인식됩니다. 이름이 소문자로 시작하면 HTML 기본 태그로 간주하므로, 브라우저는 이를 알 수 없는 HTML 요소로 처리하고 아무 것도 렌더링하지 않습니다.

### 해결 방법
컴포넌트 이름을 대문자로 시작하도록 수정하면 문제가 해결됩니다.

#### 수정된 코드

##### `loginForm.js`
```jsx
function LoginForm() {
  return (
    <form>
      <input type="text" name="username" id="username" />
      <input type="password" name="password" id="password" />
    </form>
  );
}

export default LoginForm;
```

##### `app.js`
```jsx
import "./styles.css";
import Greeter from "./Greeter";
import LoginForm from "./loginForm";
import { Dog, add } from "./Dog";

add(1, 2);

export default function App() {
  return (
    <div className="App">
      <LoginForm />
      <Greeter />
      <Dog />
    </div>
  );
}
```

### 추가 설명
React에서 컴포넌트 이름은 대문자로 시작하는 것이 관례이며, 이는 React의 동작 방식과 관련이 깊습니다. React는 소문자로 시작하는 태그를 HTML 기본 태그로 처리하기 때문에, 사용자 정의 컴포넌트를 정의할 때 대문자로 시작해야만 정상적으로 렌더링됩니다.

### 요약
- React 컴포넌트 이름은 반드시 대문자로 시작해야 합니다.
- 이름이 소문자로 시작하면 React는 이를 HTML 태그로 간주합니다.
- 문제를 해결하려면 컴포넌트 이름을 대문자로 수정하세요.

이 규칙을 준수하면 React에서 발생할 수 있는 불필요한 렌더링 오류를 방지할 수 있습니다.

