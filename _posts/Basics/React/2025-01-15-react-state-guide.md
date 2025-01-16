---
title: "React.js State: 개념과 사용법 상세 가이드"
excerpt: "React.js에서 state의 개념을 깊이 있게 이해하고, 함수형 및 클래스 컴포넌트에서 state를 관리하는 방법과 주요 특징을 코드 예제와 함께 설명합니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, State, 상태 관리]
permalink: /react/state-guide/
toc: true
toc_sticky: true
date: 2025-01-15
last_modified_at: 2025-01-15
---

React.js의 **state**는 React 컴포넌트에서 동적인 데이터를 관리하고 업데이트하기 위해 사용되는 핵심 개념 중 하나입니다. **state**는 컴포넌트의 현재 상태를 나타내며, 변경되었을 때 React는 해당 컴포넌트를 다시 렌더링(render)하여 UI를 업데이트합니다. 아래에서 React의 state에 대해 자세히 설명하겠습니다.

---

### 1. **State란?**
- **state**는 React 컴포넌트 내부에서 관리되는 **동적 데이터**를 말합니다.
- React 컴포넌트의 상태는 시간이 지남에 따라 변경될 수 있으며, 상태가 변경되면 React는 **자동으로 해당 컴포넌트를 다시 렌더링**합니다.
- 상태 데이터는 일반적으로 사용자 상호작용(클릭, 입력 등)이나 네트워크 요청, 타이머 등에 의해 변경됩니다.

---

### 2. **State와 Props의 차이**

| **State**                         | **Props**                        |
|------------------------------------|-----------------------------------|
| 컴포넌트 내부에서 관리되는 데이터  | 부모 컴포넌트로부터 전달받는 데이터 |
| 변경 가능 (mutable)                | 변경 불가능 (immutable)           |
| 초기값을 설정한 후 컴포넌트 내부에서 업데이트 가능 | 읽기 전용, 부모가 제어             |

---

### 3. **State 사용법**

#### 3.1 **useState Hook**
React의 함수형 컴포넌트에서 **state**를 관리하려면 `useState` Hook을 사용합니다.

```jsx
import React, { useState } from 'react';

function Counter() {
  // count는 state 변수, setCount는 state를 업데이트하는 함수
  const [count, setCount] = useState(0); // 초기값: 0

  return (
    <div>
      <p>현재 카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
      <button onClick={() => setCount(count - 1)}>감소</button>
    </div>
  );
}

export default Counter;
```

**설명**:
1. `useState`는 배열을 반환하며, 첫 번째 요소는 **현재 state 값**, 두 번째 요소는 **state를 업데이트하는 함수**입니다.
2. `setCount`를 호출하면 React는 해당 컴포넌트를 다시 렌더링합니다.

---

#### 3.2 **클래스 컴포넌트에서의 State**
클래스 컴포넌트에서는 `this.state`와 `this.setState`를 사용해 상태를 관리합니다.

```jsx
import React, { Component } from 'react';

class Counter extends Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 }; // 초기 state 설정
  }

  increment = () => {
    this.setState({ count: this.state.count + 1 });
  };

  decrement = () => {
    this.setState({ count: this.state.count - 1 });
  };

  render() {
    return (
      <div>
        <p>현재 카운트: {this.state.count}</p>
        <button onClick={this.increment}>증가</button>
        <button onClick={this.decrement}>감소</button>
      </div>
    );
  }
}

export default Counter;
```

---

### 4. **State 업데이트의 특징**

1. **비동기적 처리**:
   - React는 `setState`나 `useState`로 state를 업데이트할 때 **비동기적**으로 처리할 수 있습니다.
   - 따라서 상태를 업데이트한 직후에 값을 확인하려고 하면, 이전 상태가 반환될 수 있습니다.
   - 예시:
     ```jsx
     setCount(count + 1); // 이전 상태를 기준으로 업데이트
     ```

2. **이전 상태 기반 업데이트**:
   - 이전 상태를 기반으로 새로운 상태를 계산해야 할 경우, 함수형 업데이트를 사용하는 것이 안전합니다.
   ```jsx
   setCount((prevCount) => prevCount + 1);
   ```

3. **불변성 유지**:
   - React의 state는 불변성을 유지해야 합니다. 즉, 기존 객체를 직접 수정하지 않고 새로운 객체를 생성하여 업데이트해야 합니다.
   ```jsx
   // 올바르지 않은 방식
   this.state.items.push(newItem);

   // 올바른 방식
   this.setState({ items: [...this.state.items, newItem] });
   ```

---

### 5. **복잡한 State 관리**

#### 5.1 **객체 상태 관리**
state가 객체라면, 업데이트 시 기존 데이터를 보존해야 합니다.

```jsx
const [user, setUser] = useState({ name: '', age: 0 });

function updateName(newName) {
  setUser((prevUser) => ({ ...prevUser, name: newName }));
}
```

#### 5.2 **배열 상태 관리**
배열 업데이트 시에도 불변성을 유지합니다.

```jsx
const [items, setItems] = useState([]);

function addItem(newItem) {
  setItems((prevItems) => [...prevItems, newItem]);
}
```

---

### 6. **State의 최적화**

- **State를 최소화**:
  - 꼭 필요한 데이터만 state로 관리합니다. 계산 가능한 데이터는 `state`에 저장하지 않고, 필요할 때 계산합니다.
- **상위 컴포넌트와의 분리**:
  - 상태를 상위 컴포넌트에서 관리하고 props로 하위 컴포넌트에 전달하면, 더 나은 상태 관리를 할 수 있습니다.

---

### 7. **State 관련 주요 개념**

- **Lifting State Up**:
  - 여러 컴포넌트가 동일한 데이터를 공유해야 할 때, 상태를 상위 컴포넌트로 올려서 관리합니다.
- **Derived State**:
  - state를 직접 계산하지 않고, props나 다른 state를 통해 유도된 값을 사용하는 방식입니다.

---

React의 state는 컴포넌트 기반 UI를 동적으로 관리하는 데 매우 중요하며, 올바르게 사용하는 것이 성능 최적화와 유지보수에 핵심입니다. 궁금한 점이 있다면 댓글로 남겨주세요! 😊

