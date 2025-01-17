---
title: "React.js 상태 초기화 함수: 종류와 사용법"
excerpt: "React.js에서 상태 초기화 방법에 대해 살펴보고, Class Component, Function Component, useReducer를 활용한 다양한 초기화 방법을 코드 예제와 함께 설명합니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, 상태 관리, useState, useReducer]
permalink: /react/state-initialization-functions/
toc: true
toc_sticky: true
date: 2025-01-16
last_modified_at: 2025-01-16
---

React.js에서 상태 초기화 함수는 컴포넌트의 상태(state)를 초기화하거나 재설정하는 데 사용되는 로직이나 메서드를 의미합니다. 상태 초기화는 주로 컴포넌트가 처음 렌더링될 때나 특정 이벤트에 의해 상태를 다시 설정해야 할 때 이루어집니다. React의 상태 관리 방식에 따라 상태 초기화는 다음과 같은 방법으로 처리할 수 있습니다.

## 1. Class Component에서 상태 초기화
클래스 컴포넌트에서는 `constructor` 메서드에서 `this.state`를 설정하여 상태를 초기화합니다. 이후 특정 이벤트에 의해 상태를 다시 설정할 수 있습니다.

```jsx
import React, { Component } from 'react';

class MyComponent extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0, // 초기 상태
    };
  }

  resetState = () => {
    this.setState({ count: 0 }); // 상태 초기화
  };

  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.resetState}>Reset</button>
      </div>
    );
  }
}

export default MyComponent;
```

## 2. Function Component와 `useState`를 활용한 상태 초기화
함수형 컴포넌트에서는 `useState` 훅을 사용해 상태를 관리합니다. 초기 상태는 `useState`의 인자로 전달되며, 상태를 초기화하거나 재설정하려면 상태 업데이트 함수(setter)를 호출합니다.

```jsx
import React, { useState } from 'react';

function MyComponent() {
  const [count, setCount] = useState(0); // 초기 상태

  const resetState = () => {
    setCount(0); // 상태 초기화
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={resetState}>Reset</button>
    </div>
  );
}

export default MyComponent;
```

## 3. `useReducer`를 활용한 상태 초기화
복잡한 상태 관리가 필요한 경우 `useReducer`를 사용할 수 있습니다. 이 방법은 리듀서 함수와 초기 상태를 기반으로 상태를 초기화하거나 재설정합니다.

```jsx
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'reset':
      return initialState;
    default:
      return state;
  }
}

function MyComponent() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}

export default MyComponent;
```

## 4. `useState`에서 초기 상태를 함수로 설정
`useState`는 초기 상태를 함수로 설정할 수 있습니다. 이 방식은 초기화가 비용이 많이 드는 연산일 경우 성능 최적화를 위해 유용합니다. 초기화 함수는 컴포넌트가 처음 렌더링될 때만 실행됩니다.

```jsx
import React, { useState } from 'react';

function MyComponent() {
  const [count, setCount] = useState(() => {
    console.log('Initializing state');
    return 0; // 초기 상태
  });

  const resetState = () => {
    setCount(0); // 상태 초기화
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={resetState}>Reset</button>
    </div>
  );
}

export default MyComponent;
```

## 5. 리렌더링 시 상태 초기화
컴포넌트가 특정 props 또는 상태 변경에 따라 다시 렌더링될 때 상태를 초기화하려면 `useEffect`를 활용할 수 있습니다.

```jsx
import React, { useState, useEffect } from 'react';

function MyComponent({ resetTrigger }) {
  const [count, setCount] = useState(0);

  useEffect(() => {
    setCount(0); // 리렌더링 시 상태 초기화
  }, [resetTrigger]); // resetTrigger가 변경될 때 초기화

  return (
    <div>
      <p>Count: {count}</p>
    </div>
  );
}

export default MyComponent;
```

## 주요 포인트
1. 초기화는 보통 `useState`, `useReducer`, 또는 `constructor`에서 이루어집니다.
2. 초기 상태 값은 컴포넌트 외부에서 정의하거나 내부에서 동적으로 계산할 수 있습니다.
3. 이벤트 핸들러나 `useEffect` 등을 활용해 특정 조건에서 상태를 재설정합니다.
4. 복잡한 상태를 관리해야 한다면 `useReducer`를 사용하는 것이 좋습니다.

React 상태 초기화에 대해 더 알고 싶으시면 댓글로 질문을 남겨주세요! 😊

