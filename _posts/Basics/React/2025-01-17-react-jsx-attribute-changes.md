---
title: "React.js에서 JSX 속성 변경 사항 정리: className, htmlFor 등"
excerpt: "React.js에서 기존 HTML 속성과 달라지는 JSX 속성을 정리하고, 그 변경 이유와 사용 방법을 코드 예제와 함께 설명합니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, JSX, 속성 변경]
permalink: /react/jsx-attribute-changes/
toc: true
toc_sticky: true
date: 2025-01-17
last_modified_at: 2025-01-17
---

React.js에서는 기존 HTML 속성과 다른 방식으로 작성해야 하는 JSX 속성들이 있습니다. 이는 JavaScript와 HTML의 충돌을 방지하고 일관성을 유지하기 위한 설계 철학 때문입니다. 이번 글에서는 이러한 변경 사항들을 코드 예제와 함께 정리해 보겠습니다.

---

## 1. `class` → `className`

### 이유
- HTML에서 `class`는 스타일을 지정할 때 사용하는 속성이지만, JavaScript에서는 클래스 정의에 사용되는 예약어입니다.
- React는 이러한 충돌을 피하고 명확성을 유지하기 위해 `class` 대신 `className`을 사용합니다.

### 사용 예시
```jsx
// 잘못된 예시 (에러 발생)
<div class="container">Hello, React!</div>

// 올바른 예시
<div className="container">Hello, React!</div>
```

---

## 2. `for` → `htmlFor`

### 이유
- HTML에서 `for`는 `<label>` 요소가 특정 `<input>` 요소와 연결될 때 사용됩니다.
- 그러나 JavaScript에서는 `for`가 반복문에 사용되는 예약어이기 때문에 React에서 `htmlFor`로 변경되었습니다.

### 사용 예시
```jsx
// 잘못된 예시 (에러 발생)
<label for="username">Username:</label>

// 올바른 예시
<label htmlFor="username">Username:</label>
<input id="username" type="text" />
```

---

## 3. CamelCase로 작성되는 속성

### 이유
- HTML 속성은 보통 소문자로 작성되지만, React에서는 JavaScript 객체와의 일관성을 위해 CamelCase 형식을 사용합니다.

### 주요 예시
- `onclick` → `onClick`
- `tabindex` → `tabIndex`
- `spellcheck` → `spellCheck`

### 사용 예시
```jsx
<button onClick={handleClick}>Click me!</button>
<div tabIndex="0">Focusable Element</div>
<textarea spellCheck="true"></textarea>
```

---

## 4. `style` 속성: 문자열 → 객체

### 이유
- HTML에서는 `style` 속성을 문자열로 작성하지만, React에서는 JavaScript 객체를 사용하여 스타일을 정의합니다. 이는 동적 스타일링을 더 쉽게 처리할 수 있도록 설계되었습니다.

### 사용 예시
```jsx
// 잘못된 예시 (에러 발생)
<div style="color: red; font-size: 20px;">Styled Text</div>

// 올바른 예시
<div style={{ color: 'red', fontSize: '20px' }}>Styled Text</div>
```

---

## 5. boolean 속성: 명시적으로 작성

### 이유
- HTML에서는 boolean 속성을 이름만 작성하면 `true`로 간주되지만, React에서는 명시적으로 값을 지정해야 합니다.

### 사용 예시
```jsx
// 잘못된 예시 (에러 발생)
<input type="checkbox" checked>

// 올바른 예시
<input type="checkbox" checked={true} />
<input type="checkbox" checked={false} />
```

---

## 6. DOM 속성의 명칭 변경

React에서 일부 HTML 속성의 이름이 변경되거나 보완되었습니다. CamelCase가 적용되거나 기존 속성이 다른 이름으로 대체되었습니다.

### 주요 변경 사항
| HTML 속성       | React 속성         | 설명                     |
|-----------------|------------------|------------------------|
| `maxlength`     | `maxLength`      | CamelCase 적용          |
| `autocomplete`  | `autoComplete`   | CamelCase 적용          |
| `enctype`       | `encType`        | CamelCase 적용          |
| `readonly`      | `readOnly`       | CamelCase 적용          |
| `contenteditable` | `contentEditable` | CamelCase 적용       |

### 사용 예시
```jsx
<input type="text" maxLength={10} autoComplete="on" readOnly={true} />
```

---

## 7. 이벤트 핸들러: 소문자 → CamelCase

### 이유
- HTML 이벤트 속성은 소문자로 작성되지만, React에서는 CamelCase로 작성해야 합니다.
- React의 이벤트 핸들러는 JavaScript 함수로 연결됩니다.

### 사용 예시
```jsx
<input type="text" onChange={(e) => console.log(e.target.value)} />
<button onClick={handleClick}>Click me!</button>
```

---

## 8. React 전용 속성

React는 HTML에 없는 고유한 속성을 제공합니다. 주요 예는 다음과 같습니다:

### 주요 예시
- `key`: 리스트 항목을 고유하게 식별할 때 사용됩니다.
- `ref`: DOM 요소나 React 컴포넌트에 직접 접근할 때 사용됩니다.

### 사용 예시
```jsx
<ul>
  {items.map((item) => (
    <li key={item.id}>{item.name}</li>
  ))}
</ul>
```

---

## 결론
React에서 JSX 속성이 변경된 이유는 JavaScript 표준과 충돌을 방지하고 일관성을 유지하기 위함입니다. 이러한 변경 사항을 숙지하면 React 컴포넌트를 작성할 때 더 효율적이고 오류 없이 개발할 수 있습니다.

