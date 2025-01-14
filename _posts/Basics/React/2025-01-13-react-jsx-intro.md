---
title: "React.js의 JSX: 간단하고 직관적인 UI 설계"
excerpt: "React.js에서 JSX를 사용하는 이유와 특징, 내부 동작 원리, 그리고 주요 사용 방법을 알아봅니다. JSX로 더 직관적인 UI를 작성해보세요."
categories:
  - React
  - Frontend
  - JavaScript
tags:
  - [React, JSX, UI 설계, 프론트엔드]
permalink: /react/jsx-intro/
toc: true
toc_sticky: true
date: 2025-01-13
last_modified_at: 2025-01-13
---

## JSX란 무엇인가?

JSX는 **JavaScript XML**의 약자로, React.js에서 사용되는 문법 확장입니다. JSX를 사용하면 JavaScript 코드 안에서 HTML 같은 구문을 작성할 수 있어 UI를 더 직관적으로 표현할 수 있습니다. JSX는 브라우저가 이해할 수 없으므로, Babel과 같은 컴파일러가 JSX를 일반 JavaScript 코드로 변환합니다.

---

## 왜 JSX를 사용하는가?

1. **직관적인 문법**  
   JSX는 HTML과 유사한 문법을 사용하므로 개발자가 UI의 구조를 보다 쉽게 이해하고 작성할 수 있습니다.
   
2. **JavaScript와 통합**  
   JSX는 JavaScript 코드 안에서 사용되므로 동적인 데이터를 처리하거나 조건부 렌더링 등을 더 자연스럽게 구현할 수 있습니다.

3. **React의 강력한 생태계 지원**  
   JSX는 React와 함께 설계되었기 때문에 React 컴포넌트와 완벽히 통합됩니다.

---

## JSX의 주요 특징

### 1. HTML과 유사한 문법
```jsx
const element = <h1>Hello, world!</h1>;
```

### 2. JavaScript 표현식 포함 가능
중괄호 `{}`를 사용하여 JavaScript 표현식을 포함할 수 있습니다.
```jsx
const name = "John";
const greeting = <h1>Hello, {name}!</h1>;
```

### 3. HTML 속성과 유사한 속성 지정
JSX에서는 HTML 속성과 유사하게 속성을 지정하지만, camelCase 형식을 사용해야 합니다.
```jsx
const element = <button className="btn" onClick={handleClick}>Click Me</button>;
```

### 4. 태그 닫기 필수
JSX에서는 모든 태그가 닫혀 있어야 합니다. 셀프 클로징 태그도 `/`를 사용해야 합니다.
```jsx
const image = <img src="image.png" alt="Example" />;
```

### 5. JavaScript 객체를 스타일로 지정 가능
`style` 속성에는 JavaScript 객체를 사용하며, CSS 속성 이름은 camelCase로 작성합니다.
```jsx
const style = { color: 'red', fontSize: '20px' };
const element = <h1 style={style}>Styled Text</h1>;
```

---

## JSX의 내부 동작

JSX는 실제로 React의 `React.createElement()` 함수로 변환됩니다. 예를 들어:
```jsx
const element = <h1>Hello, world!</h1>;
```
위 코드는 Babel에 의해 다음과 같이 변환됩니다:
```javascript
const element = React.createElement('h1', null, 'Hello, world!');
```

---

## JSX 사용 시 주의할 점

### 1. **class와 className**
JSX에서 HTML `class` 속성은 `className`으로 작성해야 합니다.  
```jsx
<div className="container"></div>
```

### 2. **HTML과의 차이점**
- `for` 속성은 `htmlFor`로 작성해야 합니다.
- `onClick`과 같은 이벤트 핸들러는 camelCase로 작성합니다.

### 3. **조건부 렌더링**
삼항 연산자를 사용해 조건부 렌더링을 구현할 수 있습니다.
```jsx
const isLoggedIn = true;
const element = isLoggedIn ? <h1>Welcome back!</h1> : <h1>Please log in.</h1>;
```

### 4. **JSX 안에 배열 렌더링**
배열을 JSX로 렌더링할 때는 `map` 함수를 자주 사용합니다.
```jsx
const items = ['Apple', 'Banana', 'Cherry'];
const list = (
  <ul>
    {items.map(item => <li key={item}>{item}</li>)}
  </ul>
);
```

---

## 결론

JSX는 React에서 UI를 선언적으로 작성할 수 있게 해주는 강력한 도구입니다. JavaScript와 HTML을 매끄럽게 결합하여 가독성과 유지보수성을 높여줍니다. JSX의 동작과 특징을 이해하면 React 개발을 훨씬 효율적으로 진행할 수 있습니다. 😊

