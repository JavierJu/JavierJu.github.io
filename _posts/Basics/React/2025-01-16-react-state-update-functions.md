---
title: "React.js 상태 업데이트 함수: 사용법과 주의사항"
excerpt: "React.js에서 상태 업데이트 함수를 효과적으로 사용하는 방법과 주의사항을 코드 예제와 함께 설명합니다. 비동기적 동작, 함수형 업데이트, 불변성 유지 등 핵심 개념을 다룹니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, 상태 관리, useState]
permalink: /react/state-update-functions/
toc: true
toc_sticky: true
date: 2025-01-16
last_modified_at: 2025-01-16
---

## React.js 상태 업데이트 함수란?
React.js에서 상태(state)는 컴포넌트의 동적인 데이터를 관리하는 데 사용됩니다. 상태는 **불변(immutable)**해야 하며, 이를 업데이트하려면 항상 **상태 업데이트 함수**를 사용해야 합니다.

`useState` 훅은 상태를 정의하고 업데이트 함수와 함께 반환합니다.

```javascript
const [state, setState] = useState(initialState);
```

- `state`: 현재 상태 값.
- `setState`: 상태를 업데이트하는 함수.

---

## 상태 업데이트 함수 사용 방법

### 1. 새로운 값으로 설정하기
`setState(newValue)`를 호출하여 상태를 새로운 값으로 설정합니다.

#### 예제:
```javascript
setState(10); // 상태를 10으로 설정
```

### 2. 이전 상태를 기반으로 업데이트하기 (함수형 업데이트)
상태가 이전 상태에 의존하는 경우, 함수형 업데이트를 사용하는 것이 안전합니다.

#### 함수형 업데이트 예제:
```javascript
setState(prevState => prevState + 1);
```

`prevState`는 React가 자동으로 전달하는 이전 상태 값입니다.

#### 코드 전체 예제:
```javascript
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => setCount(prevCount => prevCount + 1);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

export default Counter;
```

---

## 상태 업데이트 시 주의사항

### 1. **상태 업데이트는 비동기로 동작**
`setState`는 비동기로 동작하므로, 호출 직후 상태 값을 읽어오면 변경이 반영되지 않을 수 있습니다. 예를 들어:

```javascript
setCount(count + 1);
console.log(count); // 이전 값이 출력될 수 있음
```

### 2. **불변성을 유지해야 함**
상태를 직접 수정하지 않고, 기존 상태를 복사한 후 새로운 값을 추가해야 합니다.

#### 배열 업데이트 예제:
```javascript
const [items, setItems] = useState([]);

// 올바른 방식
setItems(prevItems => [...prevItems, newItem]);

// 잘못된 방식 (직접 수정)
items.push(newItem); // ❌
setItems(items);
```

#### 객체 업데이트 예제:
```javascript
const [state, setState] = useState({ name: '', age: 0 });

// 올바른 방식
setState(prevState => ({ ...prevState, name: 'Alice' }));

// 잘못된 방식 (age가 사라짐)
setState({ name: 'Alice' }); // ❌
```

### 3. **상태는 병합되지 않음**
`setState`는 기존 상태와 병합되지 않으므로, 새 상태 객체를 완전히 정의해야 합니다.

---

## 요약
- `setState`는 상태를 업데이트하고 컴포넌트를 다시 렌더링하도록 트리거합니다.
- **함수형 업데이트**를 사용하면 이전 상태를 안전하게 참조할 수 있습니다.
- 상태를 업데이트할 때는 **불변성을 유지**해야 하며, 배열이나 객체 상태를 다룰 때 주의가 필요합니다.
- 상태는 병합되지 않으므로 필요한 모든 값을 명시적으로 포함해야 합니다.

React의 상태 관리에 대해 궁금한 점이 있다면 댓글로 질문해주세요! 😊

