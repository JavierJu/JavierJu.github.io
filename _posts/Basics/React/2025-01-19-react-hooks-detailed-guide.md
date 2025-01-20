---
title: "React.js Hooks: 자세한 설명과 예제"
excerpt: "React.js의 Hooks에 대해 자세히 알아보고, 상태 관리와 사이드 이펙트 처리를 포함한 주요 Hook의 사용법과 예제를 소개합니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, Hooks, 상태 관리, 사이드 이펙트]
permalink: /react/hooks-detailed-guide/
toc: true
toc_sticky: true
date: 2025-01-19
last_modified_at: 2025-01-19
---

React.js의 **Hooks**는 React 16.8에서 도입된 기능으로, **class 컴포넌트를 사용하지 않고도 상태 관리(state management)와 React의 라이프사이클 기능을 함수형 컴포넌트에서 사용할 수 있도록** 합니다. 이 글에서는 Hooks의 작동 원리, 주요 Hook들, 그리고 구체적인 사용 예제를 소개합니다.

## Hooks의 주요 원칙
Hooks를 사용할 때는 다음 두 가지 규칙을 반드시 따라야 합니다:

1. **Hooks는 최상위에서만 호출해야 합니다.**
   - 함수 컴포넌트의 최상위 수준에서만 호출해야 하며, 조건문이나 반복문 안에서 호출하면 안 됩니다.
   - 이는 React가 Hook의 호출 순서를 올바르게 추적하기 위해 필요합니다.

2. **React 함수 컴포넌트나 Custom Hook 내부에서만 호출해야 합니다.**
   - 일반 JavaScript 함수에서는 Hooks를 사용할 수 없습니다.

---

## 기본 Hooks

### 1. `useState`
- 컴포넌트의 **상태를 관리**하기 위한 Hook입니다.
- 상태 값과 상태를 갱신하는 함수를 반환합니다.

#### 문법:
```javascript
const [state, setState] = useState(initialValue);
```

#### 예제:
```javascript
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>현재 카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}
```

---

### 2. `useEffect`
- **사이드 이펙트(side effects)**를 처리하기 위한 Hook입니다.
- 데이터 가져오기(fetching), DOM 조작, 구독(subscription)과 같은 작업에 사용됩니다.

#### 문법:
```javascript
useEffect(() => {
  // 실행할 코드
  return () => {
    // 정리(clean-up) 코드
  };
}, [dependencies]);
```

- **의존성 배열 (dependencies)**:
  - 의존성 배열에 포함된 값이 변경될 때만 useEffect가 실행됩니다.
  - 배열이 비어 있으면 컴포넌트가 처음 렌더링될 때만 실행됩니다.

#### 예제:
```javascript
import React, { useState, useEffect } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds((prev) => prev + 1);
    }, 1000);

    return () => clearInterval(interval); // 컴포넌트가 언마운트될 때 정리
  }, []);

  return <p>경과 시간: {seconds}초</p>;
}
```

---

### 3. `useContext`
- React의 **Context API**와 함께 사용하여, 컴포넌트 트리 전체에 데이터를 쉽게 전달할 수 있습니다.

#### 문법:
```javascript
const value = useContext(MyContext);
```

#### 예제:
```javascript
import React, { useContext } from 'react';

const ThemeContext = React.createContext('light');

function ThemedComponent() {
  const theme = useContext(ThemeContext);
  return <div>현재 테마: {theme}</div>;
}

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <ThemedComponent />
    </ThemeContext.Provider>
  );
}
```

---

## 고급 Hooks

### 1. `useReducer`
- 복잡한 상태 관리 로직을 처리할 때 사용됩니다. **Redux와 비슷한 개념의 Reducer 패턴**을 사용합니다.

#### 문법:
```javascript
const [state, dispatch] = useReducer(reducer, initialState);
```

#### 예제:
```javascript
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>카운트: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>증가</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>감소</button>
    </div>
  );
}
```

---

### 2. `useMemo`
- **컴퓨팅 비용이 큰 계산을 메모이제이션**하여 성능을 최적화합니다.
- 의존성 값이 변경되지 않으면 이전 계산 값을 재사용합니다.

#### 문법:
```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

#### 예제:
```javascript
import React, { useState, useMemo } from 'react';

function ExpensiveCalculation({ number }) {
  const calculateFactorial = (n) => {
    console.log('계산 중...');
    return n <= 1 ? 1 : n * calculateFactorial(n - 1);
  };

  const factorial = useMemo(() => calculateFactorial(number), [number]);

  return <p>팩토리얼 결과: {factorial}</p>;
}
```

---

### 3. `useCallback`
- **함수의 메모이제이션**을 제공합니다.
- 함수 컴포넌트가 렌더링될 때 동일한 함수 객체를 재사용하여 불필요한 렌더링을 방지합니다.

#### 문법:
```javascript
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

#### 예제:
```javascript
import React, { useState, useCallback } from 'react';

function Child({ onClick }) {
  console.log('Child 렌더링');
  return <button onClick={onClick}>클릭</button>;
}

function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log('클릭');
  }, []);

  return (
    <div>
      <p>카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
      <Child onClick={handleClick} />
    </div>
  );
}
```

---

## Custom Hooks
- 여러 컴포넌트에서 재사용 가능한 로직을 추출하여 Custom Hook으로 만듭니다.
- 이름은 반드시 "use"로 시작해야 합니다.

#### 예제:
```javascript
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch(url)
      .then((response) => response.json())
      .then((data) => setData(data));
  }, [url]);

  return data;
}

function App() {
  const data = useFetch('https://jsonplaceholder.typicode.com/todos/1');

  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```

---

## Hooks의 장점
1. **코드 간소화**: 클래스 컴포넌트보다 간결한 문법.
2. **상태 관리와 로직 분리**: Custom Hook으로 로직을 분리해 코드 재사용성 향상.
3. **더 나은 가독성**: 비즈니스 로직을 명확히 분리 가능.
4. **테스트 용이성**: 각 Hook을 독립적으로 테스트 가능.

React Hooks는 현대적인 React 개발의 핵심 도구입니다. 각 Hook의 사용법과 예제를 활용해 프로젝트에 적합한 상태 관리 및 로직을 설계해 보세요!

