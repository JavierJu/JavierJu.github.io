---
title: "React 상태에서 배열과 객체 사용법: 기본 개념과 코드 예제"
excerpt: "React에서 배열과 객체를 상태로 사용하는 방법, 불변성을 유지하는 이유와 올바른 업데이트 패턴을 코드 예제와 함께 설명합니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, 상태 관리, 배열, 객체]
permalink: /react/state-arrays-objects/
toc: true
toc_sticky: true
date: 2025-01-16
last_modified_at: 2025-01-16
---

## React 상태에서 배열과 객체 사용법

React에서 상태(state)로 배열(array)과 객체(object)를 사용하는 것은 흔한 일입니다. 하지만 React의 상태는 불변성(immutability)을 유지해야 하므로 업데이트할 때 몇 가지 주의할 점이 있습니다. 이 글에서는 배열과 객체를 상태로 사용할 때의 기본 원칙과 패턴을 코드 예제와 함께 설명합니다.

---

## 배열(array)을 상태로 사용하는 경우

### 특징
- 배열은 여러 개의 값을 순서대로 저장하는 데이터 구조입니다.
- React 상태에서 배열을 사용하면 리스트를 렌더링하거나 데이터를 추가/삭제/수정할 때 많이 사용됩니다.

### 상태 업데이트 패턴
React의 상태는 불변성을 유지해야 하므로 배열을 직접 수정하지 않고, 새로운 배열을 만들어 업데이트합니다.

#### 예제: 배열 추가
```jsx
import React, { useState } from "react";

function ArrayStateExample() {
  const [items, setItems] = useState([]);

  const addItem = () => {
    const newItem = { id: items.length + 1, name: `Item ${items.length + 1}` };
    setItems([...items, newItem]); // 기존 배열을 복사한 뒤 새 항목 추가
  };

  return (
    <div>
      <button onClick={addItem}>Add Item</button>
      <ul>
        {items.map((item) => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}

export default ArrayStateExample;
```

#### 예제: 배열 삭제
```jsx
const removeItem = (id) => {
  setItems(items.filter((item) => item.id !== id));
};
```

#### 예제: 배열 수정
```jsx
const updateItem = (id, newName) => {
  setItems(
    items.map((item) =>
      item.id === id ? { ...item, name: newName } : item
    )
  );
};
```

---

## 객체(object)를 상태로 사용하는 경우

### 특징
- 객체는 키-값 쌍으로 데이터를 저장하며, 상태에서 여러 값을 그룹화하여 관리할 때 사용됩니다.
- 복잡한 상태를 효율적으로 다룰 수 있습니다.

### 상태 업데이트 패턴
React에서 객체를 상태로 사용할 때는 객체를 직접 수정하지 않고, 새로운 객체를 생성해 업데이트해야 합니다.

#### 예제: 객체 상태 업데이트
```jsx
import React, { useState } from "react";

function ObjectStateExample() {
  const [user, setUser] = useState({ name: "John", age: 30 });

  const updateName = () => {
    setUser({ ...user, name: "Jane" }); // 기존 객체를 복사한 후 특정 키를 변경
  };

  return (
    <div>
      <p>Name: {user.name}</p>
      <p>Age: {user.age}</p>
      <button onClick={updateName}>Change Name</button>
    </div>
  );
}

export default ObjectStateExample;
```

---

## 배열과 객체 상태 사용 시 주의점

### 1. 불변성 유지
- 상태를 직접 수정하면 React의 상태 관리 원칙이 깨져 예기치 않은 동작이 발생합니다.
- `useState`의 상태를 업데이트할 때는 스프레드 연산자(`...`)나 `map`, `filter` 등을 사용해 새로운 객체/배열을 생성합니다.

### 2. 키(key) 사용
- 배열 데이터를 렌더링할 때, React는 각 항목의 고유한 `key`가 필요합니다. 고유한 값(예: `id`)을 사용해야 성능과 버그 방지에 유리합니다.

### 3. 상태 구조화
- 상태가 너무 복잡해지지 않도록 설계합니다.
- 상태가 많아질 경우, 여러 개의 상태(`useState` 여러 개 사용)로 나누거나 `useReducer`를 사용하는 것이 좋습니다.

---

## `useReducer`로 배열/객체 상태 관리

배열이나 객체 상태가 복잡해지면 `useReducer`를 사용해 상태 관리를 간소화할 수 있습니다.

#### 예제: `useReducer`로 객체 상태 관리
```jsx
import React, { useReducer } from "react";

const initialState = { name: "John", age: 30 };

function reducer(state, action) {
  switch (action.type) {
    case "SET_NAME":
      return { ...state, name: action.payload };
    case "INCREMENT_AGE":
      return { ...state, age: state.age + 1 };
    default:
      return state;
  }
}

function ReducerExample() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Name: {state.name}</p>
      <p>Age: {state.age}</p>
      <button onClick={() => dispatch({ type: "SET_NAME", payload: "Jane" })}>
        Change Name
      </button>
      <button onClick={() => dispatch({ type: "INCREMENT_AGE" })}>
        Increment Age
      </button>
    </div>
  );
}

export default ReducerExample;
```

---

## 최적화 및 베스트 프랙티스

- **`useMemo`와 `useCallback` 사용**: 렌더링 최적화를 위해 비싼 계산이 필요한 상태를 메모이제이션합니다.
- **상태 분리**: 배열/객체의 특정 부분만 자주 변경된다면 해당 부분만 별도의 상태로 관리합니다.
- **상태 관리 라이브러리 사용**: Redux, Zustand, Jotai 등 상태 관리 라이브러리를 사용해 전역 상태를 관리합니다.

---

React에서 배열과 객체를 효율적으로 상태로 사용하는 방법을 이해하면 복잡한 UI를 더 쉽게 관리할 수 있습니다. 상황에 맞는 패턴을 선택해 React 애플리케이션을 더욱 강력하게 만들어 보세요!

