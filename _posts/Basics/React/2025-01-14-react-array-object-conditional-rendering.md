---
title: "React에서 배열 및 오브젝트 전달, 조건문, 동적 스타일, Map으로 배열 렌더링하기"
excerpt: "React에서 배열과 객체를 다루고, 조건문을 활용하며, 동적 스타일을 추가하고, map으로 배열을 렌더링하는 방법을 예제와 함께 알아봅니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, 배열, 조건문, 동적 스타일, map]
permalink: /react/array-object-conditional-rendering/
toc: true
toc_sticky: true
date: 2025-01-14
last_modified_at: 2025-01-14
---

React에서 배열과 객체를 다루고, 조건문을 사용하며, 동적 스타일을 적용하고, `map` 메서드를 활용하여 배열을 렌더링하는 방법을 코드 예제와 함께 설명합니다. 각각의 주제를 순서대로 살펴보겠습니다.

---

## **1. 배열 및 오브젝트 전달**

React 컴포넌트에 배열과 객체를 전달하려면 props를 사용합니다.

### 예제 코드
```jsx
import React from 'react';

const UserProfile = ({ user }) => {
  return (
    <div>
      <h2>{user.name}</h2>
      <p>Age: {user.age}</p>
      <p>Hobbies: {user.hobbies.join(', ')}</p>
    </div>
  );
};

const App = () => {
  const user = {
    name: "Alice",
    age: 25,
    hobbies: ["Reading", "Hiking", "Gaming"],
  };

  return <UserProfile user={user} />;
};

export default App;
```

---

## **2. 조건문 사용**

조건에 따라 다른 내용을 렌더링하거나 특정 스타일을 적용할 수 있습니다.

### 예제 코드
```jsx
import React from 'react';

const StatusMessage = ({ isLoggedIn }) => {
  return (
    <div>
      {isLoggedIn ? <p>Welcome back!</p> : <p>Please log in.</p>}
    </div>
  );
};

const App = () => {
  const isLoggedIn = true; // true면 로그인 메시지, false면 로그인 요청 메시지

  return <StatusMessage isLoggedIn={isLoggedIn} />;
};

export default App;
```

---

## **3. 동적 컴포넌트 스타일 추가**

React에서는 `style` 속성을 객체 형태로 사용하며, 조건에 따라 동적으로 변경할 수 있습니다.

### 예제 코드
```jsx
import React from 'react';

const StyledButton = ({ isPrimary }) => {
  const buttonStyle = {
    backgroundColor: isPrimary ? 'blue' : 'gray',
    color: 'white',
    padding: '10px 20px',
    border: 'none',
    borderRadius: '5px',
    cursor: 'pointer',
  };

  return <button style={buttonStyle}>{isPrimary ? 'Primary' : 'Secondary'}</button>;
};

const App = () => {
  return (
    <div>
      <StyledButton isPrimary={true} />
      <StyledButton isPrimary={false} />
    </div>
  );
};

export default App;
```

---

## **4. `map`으로 배열 렌더링**

배열의 각 요소를 `map` 메서드를 사용해 컴포넌트로 렌더링할 수 있습니다.

### 예제 코드
```jsx
import React from 'react';

const TodoList = ({ todos }) => {
  return (
    <ul>
      {todos.map((todo, index) => (
        <li key={index} style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}>
          {todo.task}
        </li>
      ))}
    </ul>
  );
};

const App = () => {
  const todos = [
    { task: 'Learn React', completed: true },
    { task: 'Build a project', completed: false },
    { task: 'Review code', completed: false },
  ];

  return <TodoList todos={todos} />;
};

export default App;
```

---

## **요약**

1. **배열 및 객체 전달**: props로 전달하고 컴포넌트에서 활용합니다.
2. **조건문**: 삼항 연산자 또는 `&&`를 사용해 동적으로 콘텐츠를 렌더링합니다.
3. **동적 스타일**: 객체로 정의된 스타일을 조건에 따라 변경합니다.
4. **`map`으로 배열 렌더링**: `map` 메서드로 각 요소를 컴포넌트로 렌더링합니다.

위의 내용을 바탕으로 React 프로젝트에서 더욱 효율적으로 UI를 구성해 보세요!

