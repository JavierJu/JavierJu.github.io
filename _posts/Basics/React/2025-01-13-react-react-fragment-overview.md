---
title: "React Fragment: 불필요한 DOM 생성 없이 요소 그룹화하기"
excerpt: "React Fragment를 사용해 여러 요소를 그룹화하는 방법과 특징, 사용 사례를 알아봅니다. 불필요한 DOM 노드를 제거해 성능을 최적화하세요."
categories:
  - React
  - Frontend
  - Web Development
tags:
  - [React, JavaScript, Frontend, Optimization]
permalink: /react/react-fragment-overview/
toc: true
toc_sticky: true
date: 2025-01-13
last_modified_at: 2025-01-13
---

## React Fragment란?

React Fragment(리액트 프래그먼트)는 React에서 여러 요소를 그룹화하여 반환할 수 있게 해주는 특수한 컴포넌트입니다. 일반적으로 JSX에서 여러 요소를 반환하려면 하나의 부모 요소로 감싸야 하지만, 이 경우 불필요한 DOM 요소가 추가될 수 있습니다. React Fragment는 이러한 문제를 해결하여 추가적인 DOM 노드를 생성하지 않고도 여러 요소를 그룹화할 수 있게 해줍니다.

## 사용 방법

### 1. `<React.Fragment>` 사용
```jsx
import React from "react";

function Example() {
  return (
    <React.Fragment>
      <h1>안녕하세요!</h1>
      <p>리액트 프래그먼트에 대해 배우고 있어요.</p>
    </React.Fragment>
  );
}

export default Example;
```

### 2. 짧은 문법: 빈 태그(`<> </>`)
```jsx
import React from "react";

function Example() {
  return (
    <>
      <h1>안녕하세요!</h1>
      <p>리액트 프래그먼트에 대해 배우고 있어요.</p>
    </>
  );
}

export default Example;
```

## React Fragment의 특징

### 1. **추가적인 DOM 노드 생성 방지**
`<React.Fragment>`나 빈 태그를 사용하면 HTML 구조에 불필요한 `<div>`와 같은 부모 노드가 추가되지 않습니다.

```jsx
// 불필요한 div로 감싸는 경우
<div>
  <h1>제목</h1>
  <p>내용</p>
</div>

// React Fragment를 사용하는 경우
<>
  <h1>제목</h1>
  <p>내용</p>
</>
```

### 2. **속성 추가 가능**
`<React.Fragment>`를 사용할 때는 속성을 추가할 수 있습니다. 예를 들어, 리스트 렌더링 시 `key` 속성을 부여할 수 있습니다.

```jsx
<React.Fragment key="uniqueKey">
  <h1>제목</h1>
  <p>내용</p>
</React.Fragment>
```

빈 태그(`<> </>`)는 속성을 지원하지 않으므로, 속성이 필요한 경우 `<React.Fragment>`를 사용해야 합니다.

## React Fragment를 사용하는 이유

1. **불필요한 부모 DOM 제거**
   - 여러 요소를 그룹화하면서도 추가적인 HTML 요소를 생성하지 않으므로 DOM이 깔끔하게 유지됩니다.

2. **성능 최적화**
   - 불필요한 DOM 노드를 줄임으로써 브라우저의 렌더링 성능을 향상시킬 수 있습니다.

3. **유연한 코드 작성**
   - JSX에서 여러 요소를 반환해야 하는 상황에서 더 직관적이고 간단하게 작성할 수 있습니다.

## 결론

React Fragment는 React 애플리케이션의 구조를 단순화하고 성능을 최적화하는 데 매우 유용한 도구입니다. 불필요한 DOM 노드를 생성하지 않고 여러 JSX 요소를 그룹화할 수 있는 React Fragment를 활용해 더욱 효율적인 코드를 작성해 보세요.

