---
title: "React.js 이벤트 핸들링: 특징과 사용 방법"
excerpt: "React.js에서 이벤트를 처리하는 방법을 알아보고, SyntheticEvent와 주요 이벤트 핸들링 기법을 코드 예제와 함께 설명합니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, 이벤트, SyntheticEvent]
permalink: /react/event-handling-guide/
toc: true
toc_sticky: true
date: 2025-01-15
last_modified_at: 2025-01-15
---

React.js에서 이벤트는 DOM 이벤트와 유사하지만, 몇 가지 중요한 차이점과 고유한 특징이 있습니다. React의 이벤트 시스템은 SyntheticEvent라는 래퍼 객체를 사용하여 이벤트의 호환성과 성능을 향상시킵니다. 아래에서 React.js 이벤트의 특징과 사용 방법을 자세히 설명합니다.

## 1. SyntheticEvent
- React는 `SyntheticEvent`라는 래퍼 객체를 사용하여 브라우저 간의 이벤트 동작을 일관성 있게 만듭니다.
- `SyntheticEvent`는 브라우저의 기본 DOM 이벤트를 추상화한 객체입니다.
- React는 모든 이벤트를 같은 방식으로 처리하기 때문에 크로스 브라우저 호환성이 보장됩니다.

## 2. React 이벤트 핸들러의 특징

### 2.1 CamelCase로 작성
React에서는 이벤트 이름을 소문자 대신 `onClick`, `onChange`처럼 CamelCase로 작성합니다.
```jsx
// 잘못된 예
<button onclick="handleClick()">Click Me</button>

// 올바른 예
<button onClick={handleClick}>Click Me</button>
```

### 2.2 JSX 문법과 함께 사용
React 이벤트 핸들러는 JSX의 속성으로 전달됩니다.
```jsx
function handleClick() {
    console.log("Button clicked!");
}

<button onClick={handleClick}>Click Me</button>
```

### 2.3 함수 참조를 전달
이벤트 핸들러로 함수 **참조**를 전달하며, 함수 호출을 전달하지 않습니다.
```jsx
// 잘못된 예
<button onClick={handleClick()}>Click Me</button>

// 올바른 예
<button onClick={handleClick}>Click Me</button>
```

### 2.4 this 컨텍스트 바인딩
클래스 컴포넌트에서는 `this` 컨텍스트를 바인딩해야 합니다.
```jsx
class App extends React.Component {
    constructor(props) {
        super(props);
        this.handleClick = this.handleClick.bind(this);
    }
    handleClick() {
        console.log('Clicked!');
    }
    render() {
        return <button onClick={this.handleClick}>Click Me</button>;
    }
}
```
함수 컴포넌트에서는 화살표 함수를 사용하여 바인딩 문제를 피할 수 있습니다.

## 3. 이벤트 객체
이벤트 핸들러는 `SyntheticEvent` 객체를 매개변수로 전달받습니다.
```jsx
function handleClick(event) {
    console.log(event.target); // 이벤트가 발생한 DOM 요소
    console.log(event.type);   // 이벤트 유형
}

<button onClick={handleClick}>Click Me</button>
```

## 4. 주요 이벤트 종류
React에서는 DOM 이벤트의 대부분을 지원하며, 대표적인 이벤트는 다음과 같습니다.

### 4.1 마우스 이벤트
- `onClick`, `onDoubleClick`, `onMouseEnter`, `onMouseLeave`, `onMouseDown`, `onMouseUp`

### 4.2 키보드 이벤트
- `onKeyDown`, `onKeyPress`, `onKeyUp`

### 4.3 폼 이벤트
- `onChange`, `onInput`, `onSubmit`, `onFocus`, `onBlur`

### 4.4 기타 이벤트
- `onScroll`, `onWheel`, `onDrag`, `onDrop`

## 5. 이벤트 핸들러에 추가 데이터 전달
이벤트 핸들러에 추가 데이터를 전달하려면 화살표 함수나 `bind`를 사용할 수 있습니다.
```jsx
function handleClick(message, event) {
    console.log(message); // 추가 데이터
    console.log(event.type); // SyntheticEvent 객체
}

<button onClick={(event) => handleClick('Hello, World!', event)}>
    Click Me
</button>
```

## 6. 기본 동작 방지와 이벤트 전파
React에서는 `event.preventDefault()`와 `event.stopPropagation()`을 사용할 수 있습니다.
```jsx
function handleSubmit(event) {
    event.preventDefault(); // 기본 동작 방지
    console.log("Form submitted");
}

<form onSubmit={handleSubmit}>
    <button type="submit">Submit</button>
</form>
```

## 7. 이벤트 핸들링 최적화

### 7.1 Inline 함수 사용 시 주의
이벤트 핸들러를 인라인으로 작성하면 렌더링할 때마다 새 함수가 생성되므로 성능에 영향을 줄 수 있습니다.
```jsx
// 비효율적인 코드
<button onClick={() => console.log("Clicked!")}>Click Me</button>
```

### 7.2 `useCallback` 사용
함수 참조를 사용하거나 `useCallback` 훅을 사용해 메모이제이션합니다.
```jsx
import React, { useCallback } from 'react';

function App() {
    const handleClick = useCallback(() => {
        console.log("Clicked!");
    }, []);

    return <button onClick={handleClick}>Click Me</button>;
}
```

---

React.js의 이벤트 시스템은 DOM 이벤트와 유사하지만, React의 가상 DOM과 통합되어 성능과 크로스 브라우저 호환성을 제공합니다. 이를 적절히 활용하면 더 효율적이고 안정적인 애플리케이션을 개발할 수 있습니다.

