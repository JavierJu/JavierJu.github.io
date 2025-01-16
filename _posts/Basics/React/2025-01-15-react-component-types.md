---
title: "React 컴포넌트 작성 방식 비교: 함수형 vs 클래스형"
excerpt: "React에서 컴포넌트를 작성하는 두 가지 주요 방식인 함수형 컴포넌트와 클래스형 컴포넌트를 비교하고, 각 방식의 특징과 사용 사례를 코드 예제와 함께 설명합니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, 함수형 컴포넌트, 클래스형 컴포넌트]
permalink: /react/component-types/
toc: true
toc_sticky: true
date: 2025-01-15
last_modified_at: 2025-01-15
---

## React 컴포넌트 작성 방식 비교: 함수형 vs 클래스형

React에서 컴포넌트를 작성하는 방식에는 **함수형 컴포넌트(Function Component)**와 **클래스형 컴포넌트(Class Component)** 두 가지가 있습니다. 이 글에서는 두 방식의 차이점, 특징, 그리고 어떤 경우에 어떤 방식을 사용하는 것이 적합한지 살펴보겠습니다.

---

### 1. 함수형 컴포넌트 (Functional Component)

#### 코드 예제
```javascript
import './App.css';

function App() {
  return (
    <div>
      <h1>Hello, Functional Component!</h1>
    </div>
  );
}

export default App;
```

#### 특징
- 컴포넌트를 **함수**로 정의합니다.
- React 16.8 이후, **Hooks**를 사용하여 상태 관리와 생명주기 로직을 처리할 수 있습니다.
- 작성이 간단하고 가독성이 좋습니다.

#### 장점
- 최신 React 개발 방식에서 선호됩니다.
- 더 간결하고 명확한 코드 작성이 가능합니다.
- React 팀이 지속적으로 최적화하고 있으며 성능 면에서 유리합니다.

---

### 2. 클래스형 컴포넌트 (Class Component)

#### 코드 예제
```javascript
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div>
        <h1>Hello, Class Component!</h1>
      </div>
    );
  }
}

export default App;
```

#### 특징
- 컴포넌트를 **클래스**로 정의합니다.
- 상태 관리와 생명주기 메서드(`componentDidMount`, `componentDidUpdate` 등)를 처리하기 위해 React 16.8 이전에 주로 사용되었습니다.

#### 장점
- 상태와 생명주기 메서드의 구조가 명확합니다.
- 기존 코드베이스에서 많이 사용되고 있습니다.

---

### 두 방식이 혼재된 이유

1. **React의 진화 과정**
   - 초기 React(16.8 이전)에는 클래스형 컴포넌트가 상태 관리와 생명주기를 처리하는 유일한 방법이었습니다.
   - React 16.8 이후, **Hooks**가 도입되면서 함수형 컴포넌트에서도 상태와 생명주기를 처리할 수 있게 되었고, 함수형 컴포넌트가 더 선호되기 시작했습니다.

2. **참고 자료의 업데이트 여부**
   - 오래된 서적이나 강의에서는 여전히 클래스형 컴포넌트를 사용하는 경우가 많습니다.

3. **프로젝트의 요구 사항**
   - 기존 코드와의 호환성을 유지하기 위해 클래스형 컴포넌트를 사용해야 하는 경우도 있습니다.

---

### 함수형 컴포넌트에서 상태 관리하기: Hooks 예제

#### 코드 예제
```javascript
import React, { useState } from 'react';
import './App.css';

function App() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}

export default App;
```

### 특징
- `useState` Hook을 사용하여 함수형 컴포넌트에서도 상태를 관리할 수 있습니다.
- 생명주기 관련 작업은 `useEffect` Hook을 통해 처리할 수 있습니다.

---

### 어떤 방식을 선택해야 할까?

1. **새 프로젝트**: 함수형 컴포넌트를 사용하고, 필요한 경우 Hooks를 적극 활용하세요.
2. **기존 프로젝트**: 프로젝트에서 이미 사용 중인 방식에 따라 코드를 작성하세요.

---

### 결론

React에서 함수형 컴포넌트와 클래스형 컴포넌트는 각기 다른 역사적 배경과 장점을 가지고 있습니다. 그러나 최신 React 개발에서는 함수형 컴포넌트와 Hooks를 사용하는 것이 더 권장되는 방식입니다. 특히 새로 시작하는 프로젝트에서는 함수형 컴포넌트를 사용하여 간결하고 유지보수 가능한 코드를 작성하는 것을 추천합니다.

